#AppDomain#
##1.配置AppDomain##
使用AppDomainSetup类为新应用程序域提供带有配置信息的公共语言运行时。创建自己的应用程序域时，最重要的ApplicationBase（它是定义应用程序的根目录，当运行时需要满足类型请求时，它在ApplicationBase属性指定的目录中探测包含该类型的程序集。）。其他AppDomainSetup属性主要由运行时宿主用于配置特殊的应用程序域。

    	using System;
		using System.Reflection;
		
		class AppDomain4
		{
		    public static void Main()
		    {
		        // Create application domain setup information.
		        AppDomainSetup domaininfo = new AppDomainSetup();
		        domaininfo.ApplicationBase = "f:\\work\\development\\latest";
		
		        // Create the application domain.
		        AppDomain domain = AppDomain.CreateDomain("MyDomain", null, domaininfo);
		
		        // Write application domain information to the console.
		        Console.WriteLine("Host domain: " + AppDomain.CurrentDomain.FriendlyName);
		        Console.WriteLine("child domain: " + domain.FriendlyName);
		        Console.WriteLine("Application base is: " + domain.SetupInformation.ApplicationBase);
		
		        // Unload the application domain.
		        AppDomain.Unload(domain);
		    }
		}
##2.卸载应用程序域##
调用AppDomain.Upload方法将其卸载。Upload方法会正常关闭指定的应用程序域。卸载过程中，没有新线程可以访问该应用程序域，并且会释放该应用程序域特定的所有数据结构。

加载到应用程序域中的所有程序集都会被移除，无法再使用。如果应用程序域中的程序集不是特定于域的，则程序集的数据会保留在内存中，直到整个进程关闭。除了关闭整个进程，没有机制可以卸载非特定于域的程序集。

.NET Framework 2.0 版中没有一个线程专用于卸载应用程序域。 这将提高可靠性，尤其是在.NET Framework 承载。 当线程调用Unload，目标域标记为要卸载。 专用的线程尝试卸载的域和域中的所有线程都将立即都中止。 如果一个线程不会中止，例如因为它执行非托管的代码，或是因为正在执行finally块，然后在一段时间后的CannotUnloadAppDomainException在最初调用的线程中引发Unload。 如果不可能最终会中止的线程结束，则目标域不卸载。 因此，在.NET Framework 2.0 版domain不能保证卸载，因为它可能不能终止正在执行的线程。

1. 卸载应用程序域后，应用程序域的内存和应用程序域内开的线程情况？
  
>     应用程序域卸载后，相应的应用程序域内的内存会一并释放掉。域内的所有线程都会终止掉


2.如果不想卸载应用程序域，又想释放内存方法？
    
> 1.可以采用AppDomain内代码执行完后，调用GC.Collect()方法去手动触发GC收集。这样每次执行完后都去调用GC.Collect()性能可能会不高。可选择定时5分钟或者10分钟或者自定义时间去触发GC.Collect()。
> 2.强制卸载应用程序域，并重新加载

