# RxCSharpSetupOnMacUsingVisualStudioCode
Using  C# Reactive Extensions from Visual Studio Code for OS X


This post describe the project setup for using C# [Rx](https://msdn.microsoft.com/en-gb/data/gg577609.aspx) from [Visual Studio Code](https://code.visualstudio.com/) running on Mac OS X.

Setup project.json with the dependencies and frameworks for Rx.

    {
      "version": "1.0.0-*",
      "description": "ConsoleApp1 Console Application",
      "authors": [ "author" ],
       "tags": [ "" ],
      "projectUrl": "",
      "licenseUrl": "",

      "dependencies": {
       "Rx-Core": "2.2.5",
       "Rx-Main": "2.2.5"
      },

      "commands": {
        "ConsoleApp1": "ConsoleApplication1"
      },

      "frameworks": {
       "dnx451": { },
       "dnxcore50": {
        "dependencies": {
          "Rx-Core": "2.2.5",
          "Rx-Main": "2.2.5",
          "System.Collections": "4.0.10-beta-23109",
          "System.Collections.Concurrent" : "4.0.10-beta-23109",
          "System.Console": "4.0.0-beta-23109",
          "System.Linq": "4.0.0-beta-23109",
          "System.Threading": "4.0.10-beta-23109",
          "Microsoft.CSharp": "4.0.0-beta-23109"
        }
       }
      } 
    } 


Run `dnu restore` from the project directory to install the Rx dependencies.

`$ dnu restore`
>Microsoft .NET Development Utility Mono-x86-1.0.0-beta6-12256

>Restoring packages for /Users/colinmoore/dev/CSharp/Reactive/project.json
  CACHE https://www.nuget.org/api/v2/
  CACHE https://www.nuget.org/api/v2/FindPackagesById()?id='Rx-Core'
  GET https://www.nuget.org/api/v2/package/Rx-Core/2.2.5
  CACHE https://www.nuget.org/api/v2/FindPackagesById()?id='Rx-Main'
  GET https://www.nuget.org/api/v2/package/Rx-Main/2.2.5
  OK https://www.nuget.org/api/v2/package/Rx-Main/2.2.5 1287ms
  OK https://www.nuget.org/api/v2/package/Rx-Core/2.2.5 1488ms
  CACHE https://www.nuget.org/api/v2/FindPackagesById()?id='Rx-Interfaces'
  GET https://www.nuget.org/api/v2/package/Rx-Interfaces/2.2.5
  GET https://www.nuget.org/api/v2/FindPackagesById()?id='Rx-Linq'
  GET https://www.nuget.org/api/v2/FindPackagesById()?id='Rx-PlatformServices'
  OK https://www.nuget.org/api/v2/package/Rx-Interfaces/2.2.5 199ms
  OK https://www.nuget.org/api/v2/FindPackagesById()?id='Rx-Linq' 448ms
  GET https://www.nuget.org/api/v2/package/Rx-Linq/2.2.5
  OK https://www.nuget.org/api/v2/FindPackagesById()?id='Rx-PlatformServices' 585ms
  GET https://www.nuget.org/api/v2/package/Rx-PlatformServices/2.2.5
  OK https://www.nuget.org/api/v2/package/Rx-Linq/2.2.5 840ms
  OK https://www.nuget.org/api/v2/package/Rx-PlatformServices/2.2.5 761ms
Installing Rx-Core.2.2.5
Installing Rx-Interfaces.2.2.5
Installing Rx-Main.2.2.5
Installing Rx-Linq.2.2.5
Installing Rx-PlatformServices.2.2.5
Writing lock file /Users/colinmoore/dev/CSharp/Reactive/project.lock.json
Restore complete, 4492ms elapsed


Sample Program using Reactive extensions.

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Reactive;
    using System.Reactive.Linq;
    using System.Reactive.Subjects;

    namespace ConsoleApplication1
    {
        class Program
        {
            static void Main(string[] args)
            {
                IObservable<int> source = Observable.Range(1, 10);
                IDisposable subscription = source.Subscribe(
                    x => Console.WriteLine("OnNext: {0}", x),
                    ex => Console.WriteLine("OnError: {0}", ex.Message),
                    () => Console.WriteLine("OnCompleted"));
                Console.WriteLine("Press ENTER to unsubscribe...");
                Console.ReadLine();
                subscription.Dispose();
            }
        }
    }

Run the program `$dnx  . run`



    OnNext: 1
    OnNext: 2
    OnNext: 3
    OnNext: 4
    OnNext: 5
    OnNext: 6
    OnNext: 7
    OnNext: 8
    OnNext: 9
    OnNext: 10
    OnCompleted
    Press ENTER to unsubscribe...
