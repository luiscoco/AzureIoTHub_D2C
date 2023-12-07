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

# 2. Build and Run the application




# 3. Verify with Azure.IoT.Explorer

Donwload and install the Azure IoT Explorer: https://github.com/Azure/azure-iot-explorer

![image](https://github.com/luiscoco/AzureIoTHub_D2C/assets/32194879/1e67bdc1-883c-4ff2-98cd-7dc129b11597)

Add a new Azure IoTHub connection in the Azure IoT Explorer

![image](https://github.com/luiscoco/AzureIoTHub_D2C/assets/32194879/19592c7b-7f59-4d40-9b16-108f20c3d5f3)

Navigate to the Azure IoTHub and copy the connection string 

![image](https://github.com/luiscoco/AzureIoTHub_D2C/assets/32194879/ae04cfd8-0b77-4223-bbdf-b98bdd7c122e)

![image](https://github.com/luiscoco/AzureIoTHub_D2C/assets/32194879/881aaeab-a48d-4e74-a71f-cb2ca1f71c66)

Or get the connection string running this command

```
az iot hub connection-string show --hub-name myIoTHubname1974 --resource-group resourcegroup1
```
Do not forget to remove the quotes "" from the connection string

![image](https://github.com/luiscoco/AzureIoTHub_D2C/assets/32194879/3b3c2ef6-2d66-428b-9952-4ef2df54c036)

Add and configure a new connection to the Azure IoTHub

![image](https://github.com/luiscoco/AzureIoTHub_D2C/assets/32194879/2257f5a2-8e5b-4a22-b7a3-43865dcd6a70)

Save the connection and we can see out device inside out Azure IoTHub. Click on the Device Id

![image](https://github.com/luiscoco/AzureIoTHub_D2C/assets/32194879/799de791-f8c0-4df3-8c3e-57351c473091)

Now we can see the device option in the left menu

![image](https://github.com/luiscoco/AzureIoTHub_D2C/assets/32194879/c675b53d-8b33-4a3e-b1d8-40c7e4ef3c10)

We are going to test sending a message from the device to the cloud (D2C), for this purpose we select the **Telemetry** option in the left menu.

![image](https://github.com/luiscoco/AzureIoTHub_D2C/assets/32194879/abdd2a54-446d-4682-b0dd-62022c28ac06)
