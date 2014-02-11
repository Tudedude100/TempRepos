package io.github.Tudedude100;

import java.util.HashMap;
import java.util.Random;

import org.bukkit.command.Command;
import org.bukkit.command.CommandSender;
import org.bukkit.entity.Player;
import org.bukkit.event.player.AsyncPlayerChatEvent;
import org.bukkit.plugin.java.JavaPlugin;

public class GuessingGame extends JavaPlugin{
	
	public HashMap<String, Integer> numPlayers = new HashMap<String, Integer>();
	
	Integer num;
	
	public void onEnable(){
		getLogger().info("YOYO DIS PLUGIN BE stARTESD");
	}
	
	public Integer genNum(){
		Random randNum = new Random();
		num = randNum.nextInt(21);
		return num;
	}
	
	public boolean onCommand(CommandSender sender, Command cmd, String label, String[] args){
		if(cmd.getName().equalsIgnoreCase("nstart")){
			if(args.length==1){
				if(sender.hasPermission("num.start")){
					if(args[0].equalsIgnoreCase("easy")){
						Integer generatedNum = genNum();
						numPlayers.put(sender.getName(), generatedNum);
						return true;
					}
				}
			}
			return true;
		}
		return false;
	}
	
	public void onPlayerChat(AsyncPlayerChatEvent e){
		Player p = e.getPlayer();
		if(numPlayers.containsKey(p.getName())){
			Integer playerNum = numPlayers.get(p);
			e.setCancelled(true);
			if(e.getMessage().equals(playerNum)){
				p.sendMessage("Congratulations, you got it right!");
			}
		}
	}

}
