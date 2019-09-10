
Usage
-----

Reboot Create Example
-----

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
            var mqttBrokerPoa = "172.18.228.29";
            var mqttBrokerPort = 1883;
            int timeOut = 120;
            int keepAlive = 10;

            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
           .WithClientId("CAE93a8573f-11f7-4902-950b-f63ee71f5685")
           .WithCSEId("cos-ins")
           .WithMqttOptions(mqttBrokerPoa, mqttBrokerPort, timeOut, keepAlive)
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

Reboot Retrieve Example
-----
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
            var mqttBrokerPoa = "172.18.228.29";
            var mqttBrokerPort = 1883;
            int timeOut = 120;
            int keepAlive = 10;

            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
           .WithClientId("CAE93a8573f-11f7-4902-950b-f63ee71f5685")
           .WithCSEId("cos-ins")
           .WithMqttOptions(mqttBrokerPoa, mqttBrokerPort, timeOut, keepAlive)
           .Build();
            
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);
            
            await deviceManagementService.ConnectToPlatformAsync();

            var retrievedRebootResult = await deviceManagementService.RetrieveRebootAsync("/cos-ins/mgo_f031687b-48b6-4448-82a5-608890390c0b");

            Console.WriteLine(retrievedRebootResult.DeviceRebootId);
        }
    }
}

```

Reboot Update Example
-----
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
            var mqttBrokerPoa = "172.18.228.29";
            var mqttBrokerPort = 1883;
            int timeOut = 120;
            int keepAlive = 10;

            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
           .WithClientId("CAE93a8573f-11f7-4902-950b-f63ee71f5685")
           .WithCSEId("cos-ins")
           .WithMqttOptions(mqttBrokerPoa, mqttBrokerPort, timeOut, keepAlive)
           .Build();
            
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);
            
            await deviceManagementService.ConnectToPlatformAsync();

            var updatedRebootResult = await deviceManagementService.UpdateRebootAsync(new DeviceRebootData
            {
                DeviceRebootId = "/cos-ins/mgo_f031687b-48b6-4448-82a5-608890390c0b",
                IsFactoryReset = true,
                Reboot = false
            });
            Console.WriteLine(updatedRebootResult.DeviceRebootId);
        }
    }
}

```

Reboot Delete Example
-----
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
             var mqttBrokerPoa = "172.18.228.29";
            var mqttBrokerPort = 1883;
            int timeOut = 120;
            int keepAlive = 10;

            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
           .WithClientId("CAE93a8573f-11f7-4902-950b-f63ee71f5685")
           .WithCSEId("cos-ins")
           .WithMqttOptions(mqttBrokerPoa, mqttBrokerPort, timeOut, keepAlive)
           .Build();
            
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);
            
            await deviceManagementService.ConnectToPlatformAsync();

            var deletedRebootResult = await deviceManagementService.DeleteRebootAsync("/cos-ins/mgo_f031687b-48b6-4448-82a5-608890390c0b");
            Console.WriteLine(deletedRebootResult.DeviceRebootId);
        }
    }
}

```

Battery Create Example
-----
```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using Platform360.Devices.SDK.Contracts.DeviceManagement;
using Platform360.Devices.SDK.OneM2M.Resources.MgmtResources;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            var mqttBrokerPoa = "172.18.228.29";
            var mqttBrokerPort = 1883;
            int timeOut = 120;
            int keepAlive = 10;

            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
           .WithClientId("CAE93a8573f-11f7-4902-950b-f63ee71f5685")
           .WithCSEId("cos-ins")
           .WithMqttOptions(mqttBrokerPoa, mqttBrokerPort, timeOut, keepAlive)
           .Build();
            
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);
            
            await deviceManagementService.ConnectToPlatformAsync();

            var createdBatteryResult = await deviceManagementService.CreateBatteryAsync(new DeviceBatteryData()
            {
                Name = "BatteryName",
                BatteryLevel = 100,
                BatteryStatus = BatteryStatus.Normal
            });
            Console.WriteLine(createdBatteryResult.BatteryId);
        }
    }
}

```

Battery Retrieve Example
-----
```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using Platform360.Devices.SDK.Contracts.DeviceManagement;
using Platform360.Devices.SDK.OneM2M.Resources.MgmtResources;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            var mqttBrokerPoa = "172.18.228.29";
            var mqttBrokerPort = 1883;
            int timeOut = 120;
            int keepAlive = 10;

            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
           .WithClientId("CAE93a8573f-11f7-4902-950b-f63ee71f5685")
           .WithCSEId("cos-ins")
           .WithMqttOptions(mqttBrokerPoa, mqttBrokerPort, timeOut, keepAlive)
           .Build();
            
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);
            
            await deviceManagementService.ConnectToPlatformAsync();

            var retrievedBatteryResult = await deviceManagementService.RetrieveBatteryAsync("/cos-ins/mgo_b6e3b697-8848-4585-af61-b2060270aba0");
            Console.WriteLine(retrievedBatteryResult.BatteryId);
        }
    }
}

```

Battery Update Example
-----
```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using Platform360.Devices.SDK.Contracts.DeviceManagement;
using Platform360.Devices.SDK.OneM2M.Resources.MgmtResources;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            var mqttBrokerPoa = "172.18.228.29";
            var mqttBrokerPort = 1883;
            int timeOut = 120;
            int keepAlive = 10;

            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
           .WithClientId("CAE93a8573f-11f7-4902-950b-f63ee71f5685")
           .WithCSEId("cos-ins")
           .WithMqttOptions(mqttBrokerPoa, mqttBrokerPort, timeOut, keepAlive)
           .Build();
            
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);
            
            await deviceManagementService.ConnectToPlatformAsync();

            var updatedBatteryResult = await deviceManagementService.UpdateBatteryAsync(new DeviceBatteryData
            {
                BatteryId = "/cos-ins/mgo_b6e3b697-8848-4585-af61-b2060270aba0",
                BatteryLevel = 101,
                BatteryStatus = BatteryStatus.Charging
            });
            Console.WriteLine(updatedBatteryResult.BatteryId);
        }
    }
}

```

Battery Delete Example
-----
```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using Platform360.Devices.SDK.Contracts.DeviceManagement;
using Platform360.Devices.SDK.OneM2M.Resources.MgmtResources;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            var mqttBrokerPoa = "172.18.228.29";
            var mqttBrokerPort = 1883;
            int timeOut = 120;
            int keepAlive = 10;

            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
           .WithClientId("CAE93a8573f-11f7-4902-950b-f63ee71f5685")
           .WithCSEId("cos-ins")
           .WithMqttOptions(mqttBrokerPoa, mqttBrokerPort, timeOut, keepAlive)
           .Build();
            
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);
            
            await deviceManagementService.ConnectToPlatformAsync();

            var deletedBatteryResult= await deviceManagementService.DeleteBatteryAsync("/cos-ins/mgo_b6e3b697-8848-4585-af61-b2060270aba0");
            Console.WriteLine(deletedBatteryResult.BatteryId);
        }
    }
}

```

Memory Create Example
-----
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
            var mqttBrokerPoa = "172.18.228.29";
            var mqttBrokerPort = 1883;
            int timeOut = 120;
            int keepAlive = 10;

            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
           .WithClientId("CAE93a8573f-11f7-4902-950b-f63ee71f5685")
           .WithCSEId("cos-ins")
           .WithMqttOptions(mqttBrokerPoa, mqttBrokerPort, timeOut, keepAlive)
           .Build();
            
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);
            
            await deviceManagementService.ConnectToPlatformAsync();

            var createdMemoryResult = await deviceManagementService.CreateMemoryAsync(new DeviceMemoryData()
            {
                Name = "Memory_" + Guid.NewGuid().ToString(),
                TotalMemory = 4096,
                AvailableMemory = 970
            });
            Console.WriteLine(createdMemoryResult.MemoryId);
        }
    }
}

```

Memory Retrieve Example
-----
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
            var mqttBrokerPoa = "172.18.228.29";
            var mqttBrokerPort = 1883;
            int timeOut = 120;
            int keepAlive = 10;

            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
           .WithClientId("CAE93a8573f-11f7-4902-950b-f63ee71f5685")
           .WithCSEId("cos-ins")
           .WithMqttOptions(mqttBrokerPoa, mqttBrokerPort, timeOut, keepAlive)
           .Build();
            
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);
            
            await deviceManagementService.ConnectToPlatformAsync();

            var retrievedMemoryResult = await deviceManagementService.RetrieveMemoryAsync("/cos-ins/mgo_b620c0c2-a88a-49f8-8232-cd49e54813ea");
            Console.WriteLine(retrievedMemoryResult.MemoryId);
        }
    }
}

```

Memory Update Example
-----
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
            var mqttBrokerPoa = "172.18.228.29";
            var mqttBrokerPort = 1883;
            int timeOut = 120;
            int keepAlive = 10;

            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
           .WithClientId("CAE93a8573f-11f7-4902-950b-f63ee71f5685")
           .WithCSEId("cos-ins")
           .WithMqttOptions(mqttBrokerPoa, mqttBrokerPort, timeOut, keepAlive)
           .Build();
            
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);
            
            await deviceManagementService.ConnectToPlatformAsync();
            
            var updatedMemoryResult = await deviceManagementService.UpdateMemoryAsync(new DeviceMemoryData()
            {
                MemoryId = "/cos-ins/mgo_b620c0c2-a88a-49f8-8232-cd49e54813ea",
                Name = "Memory_Update_" + Guid.NewGuid().ToString(),
                TotalMemory = 4097,
                AvailableMemory = 977
            });
            Console.WriteLine(updatedMemoryResult.MemoryId);
        }
    }
}

```

Memory Delete Example
-----
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
            var mqttBrokerPoa = "172.18.228.29";
            var mqttBrokerPort = 1883;
            int timeOut = 120;
            int keepAlive = 10;

            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
           .WithClientId("CAE93a8573f-11f7-4902-950b-f63ee71f5685")
           .WithCSEId("cos-ins")
           .WithMqttOptions(mqttBrokerPoa, mqttBrokerPort, timeOut, keepAlive)
           .Build();
            
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);
            
            await deviceManagementService.ConnectToPlatformAsync();

            var deletedMemoryResult = await deviceManagementService.DeleteMemoryAsync("/cos-ins/mgo_b620c0c2-a88a-49f8-8232-cd49e54813ea");
            Console.WriteLine(deletedMemoryResult.MemoryId);
        }
    }
}

```

Device Info Create Example
-----
```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using Platform360.Devices.SDK.Contracts.DeviceManagement;
using System;
using System.Collections.Generic;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            var mqttBrokerPoa = "172.18.228.29";
            var mqttBrokerPort = 1883;
            int timeOut = 120;
            int keepAlive = 10;

            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
           .WithClientId("CAE93a8573f-11f7-4902-950b-f63ee71f5685")
           .WithCSEId("cos-ins")
           .WithMqttOptions(mqttBrokerPoa, mqttBrokerPort, timeOut, keepAlive)
           .Build();
            
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);
            
            await deviceManagementService.ConnectToPlatformAsync();

            var createdPhysicalDeviceInfoResult = await deviceManagementService.CreatePhysicalDeviceInfoAsync(new DeviceGeneralInfoData
            {
                Name = "DeviceInfo_" + Guid.NewGuid().ToString(),
                DeviceType = "Health",
                Manufacturer = "kd_health",
                Model = "A1018_CreateTest",
                DeviceLabel = "Red",
                Country = "TR",
                DeviceName = "DeviceName",
                FirmwareVersion = "v1",
                HardwareVersion = "v1",
                Location = "Location",
                ManufacturerDetailsLink = "ManufacturerDetailsLink",
                ManufacturingDate = "ManufacturingDate",
                OsVersion = "OsVersion",
                PresentationURL = new List<string> { "PresentationURL" },
                Protocol = new List<string> { "Protocol" },
                SoftwareVersion = "SoftwareVersion",
                SubModel = "SubModel",
                SupportURL = new List<string> { "SupportURL" },
                SystemTime = "SystemTime"
            });
            Console.WriteLine(createdPhysicalDeviceInfoResult.PhysicalDeviceInfoId);
        }
    }
}


```

Device Info Retrieve Example
-----
```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using Platform360.Devices.SDK.Contracts.DeviceManagement;
using System;
using System.Collections.Generic;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            var mqttBrokerPoa = "172.18.228.29";
            var mqttBrokerPort = 1883;
            int timeOut = 120;
            int keepAlive = 10;

            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
           .WithClientId("CAE93a8573f-11f7-4902-950b-f63ee71f5685")
           .WithCSEId("cos-ins")
           .WithMqttOptions(mqttBrokerPoa, mqttBrokerPort, timeOut, keepAlive)
           .Build();

            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);

            await deviceManagementService.ConnectToPlatformAsync();

            var retrievedPhysicalDeviceInfoResult = await deviceManagementService.RetrievePhysicalDeviceInfoAsync("/cos-ins/mgo_cbc0c2eb-78de-4871-b1a8-a0b761820dd9");
            Console.WriteLine(retrievedPhysicalDeviceInfoResult.PhysicalDeviceInfoId);
        }
    }
}


```

Device Info Update Example
-----
```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using Platform360.Devices.SDK.Contracts.DeviceManagement;
using System;
using System.Collections.Generic;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            var mqttBrokerPoa = "172.18.228.29";
            var mqttBrokerPort = 1883;
            int timeOut = 120;
            int keepAlive = 10;

            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
           .WithClientId("CAE93a8573f-11f7-4902-950b-f63ee71f5685")
           .WithCSEId("cos-ins")
           .WithMqttOptions(mqttBrokerPoa, mqttBrokerPort, timeOut, keepAlive)
           .Build();

            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);

            await deviceManagementService.ConnectToPlatformAsync();

            var updatedPhysicalDeviceInfoResult = await deviceManagementService.UpdatePhysicalDeviceInfoAsync(new DeviceGeneralInfoData
            {
                PhysicalDeviceInfoId = "/cos-ins/mgo_cbc0c2eb-78de-4871-b1a8-a0b761820dd9",
                DeviceType = "Health_U",
                DeviceLabel = "Red_U",
                DeviceName = "DeviceName_U",
                FirmwareVersion = "v1_U",
                Location = "Location_U",
                ManufacturerDetailsLink = "ManufacturerDetailsLink_U",
                OsVersion = "OsVersion_U",
                PresentationURL = new List<string> { "PresentationURL_U" },
                Protocol = new List<string> { "Protocol_U" },
                SoftwareVersion = "SoftwareVersion_U",
                SupportURL = new List<string> { "SupportURL_U" },
                SystemTime = "SystemTime_U"
            });
            Console.WriteLine(updatedPhysicalDeviceInfoResult.PhysicalDeviceInfoId);
        }
    }
}
```

Device Info Delete Example
-----
```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using Platform360.Devices.SDK.Contracts.DeviceManagement;
using System;
using System.Collections.Generic;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            var mqttBrokerPoa = "172.18.228.29";
            var mqttBrokerPort = 1883;
            int timeOut = 120;
            int keepAlive = 10;

            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
           .WithClientId("CAE93a8573f-11f7-4902-950b-f63ee71f5685")
           .WithCSEId("cos-ins")
           .WithMqttOptions(mqttBrokerPoa, mqttBrokerPort, timeOut, keepAlive)
           .Build();

            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);

            await deviceManagementService.ConnectToPlatformAsync();

            var deletedPhysicalDeviceInfoResult = await deviceManagementService.DeletePhysicalDeviceInfoAsync("/cos-ins/mgo_cbc0c2eb-78de-4871-b1a8-a0b761820dd9");
            Console.WriteLine(deletedPhysicalDeviceInfoResult.PhysicalDeviceInfoId);
        }
    }
}

```

Area Network Info Create Example
-----
```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using Platform360.Devices.SDK.Contracts.DeviceManagement;
using System;
using System.Collections.Generic;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            var mqttBrokerPoa = "172.18.228.29";
            var mqttBrokerPort = 1883;
            int timeOut = 120;
            int keepAlive = 10;

            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
           .WithClientId("CAE93a8573f-11f7-4902-950b-f63ee71f5685")
           .WithCSEId("cos-ins")
           .WithMqttOptions(mqttBrokerPoa, mqttBrokerPort, timeOut, keepAlive)
           .Build();

            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);

            await deviceManagementService.ConnectToPlatformAsync();

            var areaNetworkInfoResult = await deviceManagementService.CreateAreaNetworkInfoAsync(new AreaNetworkInfoData
            {
                Name = "AreaNetworkInfo_" + Guid.NewGuid().ToString(),
                AreaNetworkType = "IPbased",
                ListOfDevices = new List<string> { "phone", "watch" }
            });
            Console.WriteLine(areaNetworkInfoResult.AreaNetworkInfoId);
        }
    }
}

```

Area Network Info Retrieve Example
-----
```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            var mqttBrokerPoa = "172.18.228.29";
            var mqttBrokerPort = 1883;
            int timeOut = 120;
            int keepAlive = 10;

            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
           .WithClientId("CAE93a8573f-11f7-4902-950b-f63ee71f5685")
           .WithCSEId("cos-ins")
           .WithMqttOptions(mqttBrokerPoa, mqttBrokerPort, timeOut, keepAlive)
           .Build();

            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);

            await deviceManagementService.ConnectToPlatformAsync();

            var createdAreaNetworkInfoResult = await deviceManagementService.RetrieveAreaNetworkInfoAsync("/cos-ins/mgo_4b4a1713-6b0b-4add-9f67-ccfd6ed8ce35");
            Console.WriteLine(createdAreaNetworkInfoResult.AreaNetworkInfoId);
        }
    }
}
```

Area Network Info Update Example
-----
```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using Platform360.Devices.SDK.Contracts.DeviceManagement;
using System;
using System.Collections.Generic;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            var mqttBrokerPoa = "172.18.228.29";
            var mqttBrokerPort = 1883;
            int timeOut = 120;
            int keepAlive = 10;

            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
           .WithClientId("CAE93a8573f-11f7-4902-950b-f63ee71f5685")
           .WithCSEId("cos-ins")
           .WithMqttOptions(mqttBrokerPoa, mqttBrokerPort, timeOut, keepAlive)
           .Build();

            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);

            await deviceManagementService.ConnectToPlatformAsync();

            var updatedAreaNetworkInfoResult = await deviceManagementService.UpdateAreaNetworkInfoAsync(new AreaNetworkInfoData
            {
                AreaNetworkInfoId = "/cos-ins/mgo_4b4a1713-6b0b-4add-9f67-ccfd6ed8ce35",
                AreaNetworkType = "IPbased_U",
                ListOfDevices = new List<string> { "phone_U", "watch_U" }
            });
            Console.WriteLine(updatedAreaNetworkInfoResult.AreaNetworkInfoId);
        }
    }
}
```

Area Network Info Delete Example
-----
```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            var mqttBrokerPoa = "172.18.228.29";
            var mqttBrokerPort = 1883;
            int timeOut = 120;
            int keepAlive = 10;

            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
           .WithClientId("CAE93a8573f-11f7-4902-950b-f63ee71f5685")
           .WithCSEId("cos-ins")
           .WithMqttOptions(mqttBrokerPoa, mqttBrokerPort, timeOut, keepAlive)
           .Build();

            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);

            await deviceManagementService.ConnectToPlatformAsync();

            var deletedAreaNetworkInfoResult = await deviceManagementService.DeleteAreaNetworkInfoAsync("/cos-ins/mgo_4b4a1713-6b0b-4add-9f67-ccfd6ed8ce35");
            Console.WriteLine(deletedAreaNetworkInfoResult.AreaNetworkInfoId);
        }
    }
}
```

Area Network Device Info Create Example
-----
```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using Platform360.Devices.SDK.Contracts.DeviceManagement;
using System;
using System.Collections.Generic;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            var mqttBrokerPoa = "172.18.228.29";
            var mqttBrokerPort = 1883;
            int timeOut = 120;
            int keepAlive = 10;

            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
           .WithClientId("CAE93a8573f-11f7-4902-950b-f63ee71f5685")
           .WithCSEId("cos-ins")
           .WithMqttOptions(mqttBrokerPoa, mqttBrokerPort, timeOut, keepAlive)
           .Build();

            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);

            await deviceManagementService.ConnectToPlatformAsync();

            var createdAreaNetworkDeviceInfoResult = await deviceManagementService.CreateAreaNetworkDeviceInfoAsync(new AreaNetworkDeviceInfoData
            {
                Name = "AreaNetworkDeviceInfo_" + Guid.NewGuid().ToString(),
                AreaNetworkId = "/cos-ins/mgo_4b4a1713-6b0b-4add-9f67-ccfd6ed8ce35", 
                DevID = "1",
                DevType = "1",
                ListOfNeighbors = new List<string> { "neighbor1", "neighbor2" },
                DevStatus = "DevStatus",
                SleepDuration = 1,
                SleepInterval = 5
            });
            Console.WriteLine(createdAreaNetworkDeviceInfoResult.AreaNetworkDeviceInfoId);
        }
    }
}
```

Area Network Device Info Retrieve Example
-----
```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            var mqttBrokerPoa = "172.18.228.29";
            var mqttBrokerPort = 1883;
            int timeOut = 120;
            int keepAlive = 10;

            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
           .WithClientId("CAE93a8573f-11f7-4902-950b-f63ee71f5685")
           .WithCSEId("cos-ins")
           .WithMqttOptions(mqttBrokerPoa, mqttBrokerPort, timeOut, keepAlive)
           .Build();

            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);

            await deviceManagementService.ConnectToPlatformAsync();

            var retrievedAreaNetworkDeviceInfoResult = await deviceManagementService.RetrieveAreaNetworkDeviceInfoAsync("/cos-ins/mgo_a7278ca0-fc6e-402d-9b10-97484c0a004a");
            Console.WriteLine(retrievedAreaNetworkDeviceInfoResult.AreaNetworkDeviceInfoId);
        }
    }
}
```

Area Network Device Info Update Example
-----
```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using Platform360.Devices.SDK.Contracts.DeviceManagement;
using System;
using System.Collections.Generic;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            var mqttBrokerPoa = "172.18.228.29";
            var mqttBrokerPort = 1883;
            int timeOut = 120;
            int keepAlive = 10;

            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
           .WithClientId("CAE93a8573f-11f7-4902-950b-f63ee71f5685")
           .WithCSEId("cos-ins")
           .WithMqttOptions(mqttBrokerPoa, mqttBrokerPort, timeOut, keepAlive)
           .Build();

            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);

            await deviceManagementService.ConnectToPlatformAsync();

            var updatedAreaNetworkDeviceInfoResult = await deviceManagementService.UpdateAreaNetworkDeviceInfoAsync(new AreaNetworkDeviceInfoData
            {
                AreaNetworkDeviceInfoId = "/cos-ins/mgo_a7278ca0-fc6e-402d-9b10-97484c0a004a",
                DevID = "1U",
                DevType = "1U",
                ListOfNeighbors = new List<string> { "neighbor1U", "neighbor2U" },
                DevStatus = "DevStatusU",
                SleepDuration = 2,
                SleepInterval = 3
            });
            Console.WriteLine(updatedAreaNetworkDeviceInfoResult.AreaNetworkDeviceInfoId);
        }
    }
}
```

Area Network Device Info Delete Example
-----
```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            var mqttBrokerPoa = "172.18.228.29";
            var mqttBrokerPort = 1883;
            int timeOut = 120;
            int keepAlive = 10;

            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
           .WithClientId("CAE93a8573f-11f7-4902-950b-f63ee71f5685")
           .WithCSEId("cos-ins")
           .WithMqttOptions(mqttBrokerPoa, mqttBrokerPort, timeOut, keepAlive)
           .Build();

            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);

            await deviceManagementService.ConnectToPlatformAsync();

            var deletedAreaNetworkDeviceInfoResult = await deviceManagementService.DeleteAreaNetworkDeviceInfoAsync("/cos-ins/mgo_a7278ca0-fc6e-402d-9b10-97484c0a004a");
            Console.WriteLine(deletedAreaNetworkDeviceInfoResult.AreaNetworkDeviceInfoId);
        }
    }
}
```