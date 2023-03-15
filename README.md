# LaugLib

## DiscordWebhook
Simple C# written code to send messages embeds and files using discord webhooks
<br><br>
**Code usage .Net Framework**
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

//
```

> Sending message
```CSharp

//message
hook.Send(message);

//file
hook.Send(message, new FileInfo("C:/File/Path.file"));
```
