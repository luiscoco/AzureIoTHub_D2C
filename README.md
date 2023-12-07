# How to create a  .NET 8 C# console application to send messages from D2C (from Device to Azure IoTHub)

In this example we are going to create an application to simulate the Device. 

From our application(Device) we will send a message to the Azure IoTHub

For additional info about Azure IoTHub Getting Start tutorials see the references:

https://github.com/Azure/azure-iot-explorer

https://github.com/Azure/azure-iot-sdk-csharp

https://learn.microsoft.com/en-us/azure/iot-develop/quickstart-send-telemetry-iot-hub?source=recommendations&pivots=programming-language-csharp

# 0. Prerequisites

Create an Azure IoTHub

![image](https://github.com/luiscoco/AzureIoTHub_D2C/assets/32194879/39b24370-db8e-4c4f-8721-aaf475990dcb)

![image](https://github.com/luiscoco/AzureIoTHub_D2C/assets/32194879/592433a8-0271-4640-9792-ca1eb2d5f26b)

![image](https://github.com/luiscoco/AzureIoTHub_D2C/assets/32194879/3fc5bec7-7ec5-4ede-98db-e56a4db24e97)

![image](https://github.com/luiscoco/AzureIoTHub_D2C/assets/32194879/8b4279f1-ca67-4d31-8aa5-fba4bdbde8fd)

![image](https://github.com/luiscoco/AzureIoTHub_D2C/assets/32194879/5655829e-266a-4ec1-aa47-34f64fbf5c31)

![image](https://github.com/luiscoco/AzureIoTHub_D2C/assets/32194879/5b541440-3647-4957-aeb1-0ee199acff00)

![image](https://github.com/luiscoco/AzureIoTHub_D2C/assets/32194879/fe203395-6363-41b7-b54a-0996afd623cd)

Create a Device inside the previouly created Azure IoTHub

![image](https://github.com/luiscoco/AzureIoTHub_D2C/assets/32194879/4bdcdc94-9cf9-4ac1-8ebf-d7f4247b7182)

![image](https://github.com/luiscoco/AzureIoTHub_D2C/assets/32194879/c40873c0-3e99-45e6-9984-963944752af9)

![image](https://github.com/luiscoco/AzureIoTHub_D2C/assets/32194879/82ba2fab-e5d5-41b0-897e-5cc2699b2209)

# 1. Create a .NET 8 C# console application in Visual Studio 2022 Community Edition

Run Visual Studio 2022 and follow these steps

![image](https://github.com/luiscoco/AzureIoTHub_D2C/assets/32194879/f17c8fa8-e42f-4eca-9a72-233d67eecc91)

![image](https://github.com/luiscoco/AzureIoTHub_D2C/assets/32194879/071815fd-51bc-4819-ac5f-af354dcbf484)

![image](https://github.com/luiscoco/AzureIoTHub_D2C/assets/32194879/80159071-238b-4948-b453-a1a3c437afa4)

![image](https://github.com/luiscoco/AzureIoTHub_D2C/assets/32194879/19868d8c-5de8-4611-968e-431158e2071e)

Load with Nuget the library: **Microsoft.Azure.Devices.Client**

![image](https://github.com/luiscoco/AzureIoTHub_D2C/assets/32194879/b47b6a43-66e3-415a-abb8-089a8beb50d7)

# 2. Input the application source code

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

# 3. Build and Run the application and Verify it with Azure.IoT.Explorer

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

![image](https://github.com/luiscoco/AzureIoTHub_D2C/assets/32194879/20b45185-c7f5-4fea-9ec1-65175ecc68ae)

It is the time to start the C# console application to send a message from our application(our simulated device) to the Azure IoTHub

![image](https://github.com/luiscoco/AzureIoTHub_D2C/assets/32194879/1f2a6981-8fb4-4e7f-aebe-233728b0a6e6)
