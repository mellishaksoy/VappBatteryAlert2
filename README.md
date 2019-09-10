
Usage
-----

We have put some self-explanatory examples in the *Examples* project, but here is a quick breakdown on how it works. First, you need to set up a **MessageBird.Client**. Be sure to replace **YOUR_ACCESS_KEY** with something real.

```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using Platform360.Devices.SDK.Contracts.DeviceManagement;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
           .WithClientId("CAE93a8573f-11f7-4902-950b-f63ee71f5685")
           .WithCSEId("cos-ins")
           .WithMqttOptions("172.18.228.29", 1883, 12000, 100000)
           .Build();
            
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);
            
            await deviceManagementService.ConnectToPlatformAsync();
            
            var createdRebootResult = await deviceManagementService.CreateRebootAsync(new DeviceRebootData
            {
                Name = "RebootName",
                IsFactoryReset = true,
                Reboot = false
            });

            Console.WriteLine(createdRebootResult.DeviceRebootId);
        }
    }
}

```
