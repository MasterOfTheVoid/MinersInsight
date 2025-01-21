package com.example.xraymod;

import net.minecraft.client.Minecraft;
import net.minecraft.client.settings.KeyBinding;
import net.minecraft.util.BlockRenderLayer;
import net.minecraftforge.client.event.RenderWorldLastEvent;
import net.minecraftforge.common.MinecraftForge;
import net.minecraftforge.fml.common.Mod;
import net.minecraftforge.fml.common.event.FMLInitializationEvent;
import net.minecraftforge.fml.common.eventhandler.SubscribeEvent;
import org.apache.commons.lang3.ArrayUtils;
import org.lwjgl.input.Keyboard;

@Mod(modid = XrayMod.MODID, name = XrayMod.NAME, version = XrayMod.VERSION)
public class XrayMod {
    public static final String MODID = "xraymod";
    public static final String NAME = "Xray Mod";
    public static final String VERSION = "1.1";

    private KeyBinding toggleXray;
    private boolean xrayEnabled = false;

    @Mod.EventHandler
    public void init(FMLInitializationEvent event) {
        toggleXray = new KeyBinding("Toggle Xray", Keyboard.KEY_X, "Xray Mod");
        Minecraft.getMinecraft().gameSettings.keyBindings = ArrayUtils.add(Minecraft.getMinecraft().gameSettings.keyBindings, toggleXray);
        MinecraftForge.EVENT_BUS.register(this);
    }

    @SubscribeEvent
    public void onRenderWorldLast(RenderWorldLastEvent event) {
        if (toggleXray.isPressed()) {
            xrayEnabled = !xrayEnabled;
            updateBlockLayers(xrayEnabled);
        }
    }

    private void updateBlockLayers(boolean enableXray) {
        for (BlockRenderLayer layer : BlockRenderLayer.values()) {
            if (enableXray) {
                if (layer != BlockRenderLayer.TRANSLUCENT) {
                    Minecraft.getMinecraft().renderGlobal.setBlockLayerRenderers(layer, false);
                }
            } else {
                Minecraft.getMinecraft().renderGlobal.setBlockLayerRenderers(layer, true);
            }
        }
    }
}

