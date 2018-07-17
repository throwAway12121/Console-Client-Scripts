# Console-Client-Scripts
Minecraft Console Client was created by https://github.com/ORelio/

--INVITE SCRIPT START--
//MCCScript 1.0
MCC.LoadBot(new Inviter());
 
//MCCScript Extensions
 
class Inviter : ChatBot
{
    int checkCount = 0;
    List<String> players = new List<String>(); //The C# List allows for more powerful arrays
    bool stopInviting = false;
 
    public Inviter()
    {
        //Constructor for class Inviter
        LogToConsole("Looking for new players...");
    }
//EDIT 1---------------------------------------------------------
    
    public override void Initialize()
    {
        LogToConsole("Sucessfully Initialized!");
	      LogToConsole("Connecting To Towny...");	
	      SendText("/towny");

	      Thread.Sleep(3000);
	      SendText("/nickname &1J&2a&3k&4e&0BOT");
	      SendText("/tc I am afk, please do not message me.");
	      SendText("/afk");
	      LogToConsole("Finished!");
    }

    public override void GetText(string text) //This is an overrided method, meaning it writes over the existing GetText() method from the client's API
    {
        string textv = GetVerbatim(text); //GetVerbatim() is a method from the API that removes color coding from a given string
//EDIT 2---------------------------------------------------------
        if (stopChecking == false) {
	          if (textv.Contains("/shutdown")) {
                if (textv.Contains("/confirm")) {
	                  SendText("/nickname &1J&2a&3k&4e");
                    bool stopChecking = true;
	                  LogToConsole("Stopped!");
                }
	          }
        } else if (textv.Contains("/startup")) {
            if (textv.Contains("/startconfirm")) {
	              SendText("/nickname &1J&2a&3k&4e&0BOT");
                bool stopChecking = false;
                SendText("/tc I am afk, please do not message me.");
	              SendText("/afk");
	              LogToConsole("Started!");
            }
	      }
	      if (stopInviting == false) {
            if (textv.Contains("Welcome to Imperial Towny") && !textv.Contains("»")) {
            players.Add(textv.Substring(textv.IndexOf(",") + 2, textv.IndexOf("!")-textv.IndexOf(",")-2));
            } else if (textv.Contains(" invited ") && textv.Contains(" to the town.") && !textv.Contains("»")) {
            players.Remove(textv.Substring(textv.IndexOf(" invited ")+9, textv.IndexOf(" to the town.")-textv.IndexOf(" invited ")-9));
            } else if (textv.Contains("[Towny]") && textv.Contains(" is offline") && !textv.Contains("»")) {
            players.Remove(textv.Substring(8, textv.IndexOf(" is offline")-8));
            } else if (textv.Contains("[Towny]") && textv.Contains(" is already a part of ") && !textv.Contains("»")) {
            players.Remove(textv.Substring(8, textv.IndexOf(" is already")-8));
            }
        }
    }
 
    public override void Update() //Another overrided method from the API, basically gets called constantly
    {
        if (stopChecking == false) {
            if (checkCount > 200) {
                checkCount = 0;
                for (int i = 0; i < players.Count; i++) { //Looping through all players added to array...
                    if (players[i].Contains("]")) {
                        players[i] = players[i].Substring(players[i].IndexOf("]")+2);
                    }
                    SendText("/town add " + players[i]);
//EDIT 3---------------------------------------------------------
		                SendText("/msg " + players[i] + "Join CrabCove " + players[i] + ", it's better than First!");
		                SendText("/afk");
                       
                    if (players.Count > 1) Thread.Sleep(300);
               }
            } else {
                checkCount++;
            }
        }
    }
}
