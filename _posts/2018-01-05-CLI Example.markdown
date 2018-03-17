---
layout:       post
title:        "CLI/C++"
subtitle:     "CLI/C++ Example"
date:         2018-01-05 12:00:00
author:       "Gary"
header-img:   "img/post-bg-miui6.jpg"
header-mask:  0.3
catalog:      false
multilingual: false
comments:     true
tags:
    - WPF
---

### Convert a CLR string to char*
```C++
#include "msclr\marshal_cppstd.h"
 
using namespace msclr::interop;
 
marshal_context context;
 
System::String ^FileName = "D:\\1.xml";
const char * _FileName =  context.marshal_as<const char*>(FileName);
```

### Project Example

```C++
#pragma once
 
#include "stdafx.h"
#include "AMESimLibs.h"
#include "ame_api.h"
#include "ame_api_types.h"
#include "msclr\marshal_cppstd.h"
 
using namespace System::Xml;
 
namespace AMESimLibs
{
    public ref class AMEMethod
    {
    public:
        AMEMethod();
        ~AMEMethod();
        void IterationFiles(System::String ^FileName,
            System::String^ XMLFilePath);
        void Simulation(System::String ^FileName,
            array<System::String^, 2> ^Modify,
            System::String ^Target,
            System::String ^StopTime,
            System::String ^Intervals,
            [System::Runtime::InteropServices::OutAttribute] array<System::Double, 2> ^% Result);
    private:
    };
 
    AMEMethod::AMEMethod()
    {
        if (AMEIsOk(AMEInitAPI(false)))
            return;
    }
 
    AMEMethod::~AMEMethod()
    {
        AMECloseAPI();
    }
 
    void AMEMethod::IterationFiles(System::String ^FileName,
        System::String ^XMLFilePath)
    {
        msclr::interop::marshal_context context;
        XmlDocument ^document = gcnew XmlDocument;
        XmlElement ^root = document->CreateElement("ArrayOfComponent");
        root->SetAttribute("xmlns:xsd", "http://www.w3.org/2001/XMLSchema");
        root->SetAttribute("xmlns:xsi", "http://www.w3.org/2001/XMLSchema-instance");
 
        if (!AMEIsOk(AMEOpenAmeFile(context.marshal_as<const char*>(FileName))))
            return;
        System::Console::WriteLine("File Is Open");
 
        AMECompLineFlag ComLineFlag = ame_compline_all;
        AMECompLineIteration * CompLineIteration = AMECreateCompLineIteration(ComLineFlag);
        AMECompLine * CompLine = NULL;
        while ((CompLine = AMEGetNextCompLine(CompLineIteration)) != NULL)
        {
            // please note the difference from AMESIsComponent and AMEIsComponent
            // Get Component Name
            XmlElement ^component = document->CreateElement("Component");
            char * ComponentName = NULL;
            AMESCopyAliasPath(CompLine, &ComponentName);
            component->SetAttribute("Name", %System::String(ComponentName));
            // Get Component Type
            if (AMESIsComponent(CompLine))
            {
                component->SetAttribute("Type", "Component");
            }
            else
            {
                component->SetAttribute("Type", "Line");
            }
            XmlElement ^ Items = document->CreateElement("Items");
            AMEParVarFlag ParVarFlag = ame_parvar_all;
            AMEParVarIteration * ParVarIterations = AMESCreateParVarIteration(CompLine, ParVarFlag);
            AMEParVar * ParVar = NULL;
            while ((ParVar = AMEGetNextParVar(ParVarIterations)) != NULL)
            {
                XmlElement ^ Item = document->CreateElement("ParameterItem");
                char * ParVarName = NULL;
                AMESCopyDataPath(ParVar, &ParVarName);
                Item->SetAttribute("Name", %System::String(ParVarName));
                if (AMESIsParameter(ParVar))
                {
                    Item->SetAttribute("Type", "Parameter");
 
                    char * ParValue, *ParUnit, *ParTitle = NULL;
 
                    AMESGetParameterValue(ParVar, &ParValue, &ParUnit);
                    Item->SetAttribute("Value", %System::String(ParValue));
                    Item->SetAttribute("Unit", %System::String(ParUnit));
 
                    AMESGetParameterInfos(ParVar, NULL, &ParTitle, NULL);
                    Item->SetAttribute("Description", %System::String(ParTitle));
                }
                else
                {
                    Item->SetAttribute("Type", "Variable");
 
                    char * VarTitle, *VarUnit = NULL;
 
                    AMESGetVariableInfos(ParVar, NULL, NULL, &VarTitle, &VarUnit);
                    Item->SetAttribute("Description", %System::String(VarTitle));
                    Item->SetAttribute("Unit", %System::String(VarUnit));
                }
                Items->AppendChild(Item);
            }
            // end while
            component->AppendChild(Items);
            root->AppendChild(component);
        };
        document->AppendChild(root);
        document->Save(XMLFilePath);
        System::Console::WriteLine("Export Finished");
        AMECloseCircuit();
    }
 
    void AMEMethod::Simulation(System::String ^FileName,
        array<System::String^, 2> ^Modify,
        System::String ^Target,
        System::String ^StopTime,
        System::String ^Intervals,
        [System::Runtime::InteropServices::OutAttribute] array<System::Double, 2> ^% Result)
    {
        msclr::interop::marshal_context context;
        // Open File
        if (!AMEIsOk(AMEOpenAmeFile(context.marshal_as<const char*>(FileName))))
            return;
        System::Console::WriteLine("File Is Open");
        // Set Parameter Value
        int nLength = Modify->GetLength(0);
        for (int i = 0; i < nLength; i++)
        {
            if (!AMEIsOk(AMESetParameterValue(context.marshal_as<const char*>(Modify[i, 0]), context.marshal_as<const char*>(Modify[i, 1]))))
                return;
        }
        // Set Run Parameter
        AMESetRunParameter("stop_time_s", context.marshal_as<const char*>(StopTime));
        AMESetRunParameter("interval_s", context.marshal_as<const char*>(Intervals));
        AMESetRunParameter("discontinuity_printout", "0");
        // Run Simulation
        AMERunSimulation();
        System::Console::WriteLine("Simulation Succeed");
 
        unsigned int Count = 0;
        double *times, *Variables = NULL;
 
        AMEGetVariableValues(context.marshal_as<const char*>(Target), Count, &times, &Variables);
        System::Console::WriteLine("Simulation OutPut Count  " + Count);
        Result = gcnew array<System::Double, 2>(Count, 2);
        for (unsigned RowIdx = 0; RowIdx < Count; RowIdx++)
        {
            Result[RowIdx, 0] = times[RowIdx];
            Result[RowIdx, 1] = Variables[RowIdx];
        }
        AMECloseCircuit();
    }
}
```
