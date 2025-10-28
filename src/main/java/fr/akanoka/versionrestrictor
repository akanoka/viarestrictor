package fr.akanoka.versionrestrictor;

import com.viaversion.viaversion.api.Via;
import com.viaversion.viaversion.api.protocol.version.ProtocolVersion;
import org.bukkit.Bukkit;
import org.bukkit.ChatColor;
import org.bukkit.entity.Player;
import org.bukkit.event.EventHandler;
import org.bukkit.event.Listener;
import org.bukkit.event.player.PlayerJoinEvent;
import org.bukkit.plugin.java.JavaPlugin;

import java.util.HashSet;
import java.util.Set;

public class VersionRestrictor extends JavaPlugin implements Listener {

    private Set<Integer> allowedProtocols;
    private String kickMessage;

    @Override
    public void onEnable() {
        saveDefaultConfig();
        reloadConfig();
        loadConfigData();

        // Enregistrer l'événement pour la vérification des versions
        getServer().getPluginManager().registerEvents(this, this);

        getLogger().info("VersionRestrictor activé avec " + allowedProtocols.size() + " versions autorisées.");
    }

    private void loadConfigData() {
        allowedProtocols = new HashSet<>();
        for (String s : getConfig().getStringList("allowed_versions")) {
            try {
                allowedProtocols.add(Integer.parseInt(s));
            } catch (NumberFormatException e) {
                getLogger().warning("Protocole invalide dans config.yml: " + s);
            }
        }
        kickMessage = ChatColor.translateAlternateColorCodes('&', getConfig().getString("kick_message"));
    }

    @Override
    public void onDisable() {
        getLogger().info("VersionRestrictor désactivé.");
    }

    // Permet de recharger la configuration à la volée
    public void reloadPluginConfig() {
        reloadConfig();
        loadConfigData();
        getLogger().info("VersionRestrictor configuration rechargée.");
    }

    // Vérification des joueurs au join
    @EventHandler
    public void onPlayerJoin(PlayerJoinEvent event) {
        Player player = event.getPlayer();

        // Scheduler 1 tick pour que ViaVersion détecte la version du joueur
        Bukkit.getScheduler().runTask(this, () -> {
            int protocol = Via.getAPI().getPlayerVersion(player.getUniqueId());
            String mcVersion = "Unknown";

            if (protocol != -1) {
                try {
                    ProtocolVersion pv = ProtocolVersion.getProtocol(protocol);
                    if (pv != null) mcVersion = pv.getName().split("-")[0]; // Version simplifiée
                } catch (Exception ignored) {}
            }

            // Vérifie si le protocole est autorisé
            if (!allowedProtocols.contains(protocol)) {
                String message = kickMessage
                        .replace("%version%", String.valueOf(protocol))
                        .replace("%mcversion%", mcVersion);

                player.kickPlayer(ChatColor.translateAlternateColorCodes('&', message));

                if (getConfig().getBoolean("log_kicks", true)) {
                    getLogger().info("Joueur " + player.getName() + " kické pour version " + mcVersion + " (" + protocol + ")");
                }
            }
        });
    }
}
