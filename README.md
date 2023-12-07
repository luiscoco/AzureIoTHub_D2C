# How to create a C# console .NET 8 application to send messages from D2C (from Device to Azure IoTHub)

https://github.com/Azure/azure-iot-explorer

https://github.com/Azure/azure-iot-sdk-csharp

https://learn.microsoft.com/en-us/azure/iot-develop/quickstart-send-telemetry-iot-hub?source=recommendations&pivots=programming-language-csharp

# 0. Prerequisites

Create an Azure IoTHub

Create a Device inside the previouly created Azure IoTHub

To get the Device connection string run this command

```
az iot hub connection-string show --hub-name myIoTHubname1974 --resource-group resourcegroup1
```

# 1. Console application to send messages to Azure IoTHub

```csharp
using Microsoft.Azure.Devices.Client;
using System.Text;

string ConnectionString = "HostName=myIoTHubname1974.azure-devices.net;DeviceId=myDevice;SharedAccessKey=EzLbRGucSovGeSzk8WcfIvDuTqk752tpRAIoTO9Zbfk=";

var message = "Hello from Azure IoT Hub!";
await SendMessageToIoTHubAsync(message);

async Task SendMessageToIoTHubAsync(string message)
{
    var deviceClient = DeviceClient.CreateFromConnectionString(ConnectionString, TransportType.Mqtt);

    var payload = new Message(Encoding.UTF8.GetBytes(message));

    try
    {
        await deviceClient.SendEventAsync(payload);
        Console.WriteLine($"Message sent: {message}");
    }
    catch (Exception ex)
    {
        Console.WriteLine($"Error sending message: {ex.Message}");
    }
    finally
    {
        await deviceClient.CloseAsync();
    }
}
```

