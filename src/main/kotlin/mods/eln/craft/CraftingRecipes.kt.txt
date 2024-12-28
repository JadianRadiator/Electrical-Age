package mods.eln.craft

import cpw.mods.fml.common.registry.EntityRegistry
import cpw.mods.fml.common.registry.GameRegistry
import mods.eln.Eln
import mods.eln.entity.ReplicatorEntity
import mods.eln.i18n.I18N
import mods.eln.misc.Recipe
import mods.eln.misc.Utils.addSmelting
import mods.eln.misc.Utils.areSame
import mods.eln.misc.Utils.println
import net.minecraft.init.Blocks
import net.minecraft.init.Items
import net.minecraft.item.ItemStack
import net.minecraft.item.crafting.CraftingManager
import net.minecraft.item.crafting.IRecipe
import net.minecraft.launchwrapper.LogWrapper
import net.minecraftforge.oredict.OreDictionary
import net.minecraftforge.oredict.ShapedOreRecipe
import net.minecraftforge.oredict.ShapelessOreRecipe
import java.util.*
import kotlin.collections.HashSet

object CraftingRecipes {

    fun itemCrafting() {

        //
        registerReplicator()


        //
        recipeEnergyConverter()
        recipeComputerProbe()

        recipeArmor()
        recipeTool()

        recipeGround()
        recipeElectricalSource()
        recipeElectricalCable()
        recipeThermalCable()
        recipeLampSocket()
        recipeLampSupply()
        recipePowerSocket()
        recipePassiveComponent()
        recipeSwitch()
        recipeWirelessSignal()
        recipeElectricalRelay()
        recipeElectricalDataLogger()
        recipeElectricalGateSource()
        recipeElectricalBreaker()
        recipeFuses()
        recipeElectricalVuMeter()
        recipeElectricalEnvironmentalSensor()
        recipeElectricalRedstone()
        recipeElectricalGate()
        recipeElectricalAlarm()
        recipeSixNodeCache()
        recipeElectricalSensor()
        recipeThermalSensor()
        recipeSixNodeMisc()


        recipeTurret()
        recipeMachine()
        recipeChips()
        recipeTransformer()
        recipeHeatFurnace()
        recipeTurbine()
        recipeBattery()
        recipeElectricalFurnace()
        recipeAutoMiner()
        recipeSolarPanel()

        recipeThermalDissipatorPassiveAndActive()
        recipeElectricalAntenna()
        recipeEggIncubator()
        recipeBatteryCharger()
        recipeTransporter()
        recipeWindTurbine()
        recipeFuelGenerator()

        recipeGeneral()
        recipeHeatingCorp()
        recipeRegulatorItem()
        recipeLampItem()
        recipeProtection()
        recipeCombustionChamber()
        recipeFerromagneticCore()
        recipeIngot()
        recipeDust()
        recipeElectricalMotor()
        recipeSolarTracker()
        recipeDynamo()
        recipeWindRotor()
        recipeMeter()
        recipeElectricalDrill()
        recipeOreScanner()
        recipeMiningPipe()
        recipeTreeResinAndRubber()
        recipeRawCable()
        recipeGraphite()
        recipeMiscItem()
        recipeBatteryItem()
        recipeElectricalTool()
        recipePortableCapacitor()

        recipeFurnace()
        recipeArcFurnace()
        recipeMacerator()
        recipeCompressor()
        recipePlateMachine()
        recipeMagnetizer()
        recipeFuelBurnerItem()
        recipeDisplays()

        recipeECoal()

        recipeGridDevices(Eln.oreNames)
        recipeMaceratorModOres()
        craftBrush()
        val cal: Calendar = Calendar.getInstance()
        val month: Int = cal.get(Calendar.MONTH) + 1
        if(month == 12 ) {
            recipeChristmas()
        }

        checkRecipe()
    }

    private fun findItemStack(name: String): ItemStack {
        return Eln.findItemStack(name, 1)
    }

    private fun firstExistingOre(vararg oreNames: String): String {
        for (oreName in oreNames) {
            if (OreDictionary.doesOreNameExist(oreName)) {
                return oreName
            }
        }
        return ""
    }

    private fun checkRecipe() {
        println("No recipe for ")
        for (d in Eln.sixNodeItem.subItemList.values) {
            val stack = d?.newItemStack()
            if (!recipeExists(stack)) {
                println("  " + d?.name)
            }
        }
        for (d in Eln.transparentNodeItem.subItemList.values) {
            val stack = d?.newItemStack()
            if (!recipeExists(stack)) {
                println("  " + d?.name)
            }
        }
        for (d in Eln.sharedItem.subItemList.values) {
            val stack = d.newItemStack()
            if (!recipeExists(stack)) {
                println("  " + d.name)
            }
        }
        for (d in Eln.sharedItemStackOne.subItemList.values) {
            val stack = d.newItemStack()
            if (!recipeExists(stack)) {
                println("  " + d.name)
            }
        }
    }

    private fun recipeExists(stack: ItemStack?): Boolean {
        if (stack == null) return false
        val list = CraftingManager.getInstance().recipeList
        for (o in list) {
            if (o is IRecipe) {
                if (o.recipeOutput == null) continue
                if (areSame(stack, o.recipeOutput)) return true
            }
        }
        return false
    }

    private fun craftBrush() {
        val emptyStack: ItemStack = findItemStack("White Brush")
        Eln.whiteDesc!!.setLife(emptyStack, 0)
        for (idx in 0..15) {
            addShapelessRecipe(emptyStack.copy(), ItemStack(Blocks.wool, 1, idx), findItemStack("Iron Cable"))
        }
        for (idx in 0..15) {
            val name = Eln.brushSubNames[idx]
            addShapelessRecipe(Eln.findItemStack(name, 1), ItemStack(Items.dye, 1, idx), emptyStack.copy())
        }
    }

    private fun recipeGround() {
        addRecipe(findItemStack("Ground Cable"), " C ", " C ", "CCC", 'C', findItemStack("Copper Cable"))
    }

    private fun recipeElectricalSource() {
    }

    private fun recipeElectricalCable() {
        addRecipe(
            Eln.instance.signalCableDescriptor.newItemStack(2),
            "R",
            "C",
            "C",
            'C',
            findItemStack("Iron Cable"),
            'R',
            "itemRubber"
        )
        addRecipe(
            Eln.instance.lowVoltageCableDescriptor.newItemStack(2),  //Low Voltage Cable
            "R", "C", "C", 'C', findItemStack("Copper Cable"), 'R', "itemRubber"
        )
        addRecipe(
            Eln.instance.lowCurrentCableDescriptor.newItemStack(4), "RC ", "   ", "   ", 'C', findItemStack("Copper Cable"),
            'R', "itemRubber"
        )
        addRecipe(
            Eln.instance.meduimVoltageCableDescriptor.newItemStack(2),  //Meduim Voltage Cable (Medium Voltage Cable)
            "R", "C", 'C', Eln.instance.lowVoltageCableDescriptor.newItemStack(1), 'R', "itemRubber"
        )
        addRecipe(
            Eln.instance.mediumCurrentCableDescriptor.newItemStack(4), "RC ", "RC ", "   ", 'C', findItemStack(
                "Copper " +
                        "Cable"
            ), 'R', "itemRubber"
        )
        addRecipe(
            Eln.instance.highVoltageCableDescriptor.newItemStack(2),  //High Voltage Cable
            "R", "C", 'C', Eln.instance.meduimVoltageCableDescriptor.newItemStack(1), 'R', "itemRubber"
        )
        addRecipe(
            Eln.instance.highCurrentCableDescriptor.newItemStack(4), "RC ", "RC ", "RC ", 'C', "ingotCopper", 'R',
            "itemRubber"
        )
        addRecipe(
            Eln.instance.signalCableDescriptor.newItemStack(12),  //Signal Wire
            "RRR", "CCC", "RRR", 'C', ItemStack(Items.iron_ingot), 'R', "itemRubber"
        )
        addRecipe(
            Eln.instance.signalBusCableDescriptor.newItemStack(1), "R", "C", 'C', Eln.instance.signalCableDescriptor.newItemStack(1), 'R',
            "itemRubber"
        )
        addRecipe(
            Eln.instance.lowVoltageCableDescriptor.newItemStack(12), "RRR", "CCC", "RRR", 'C', "ingotCopper", 'R',
            "itemRubber"
        )
        addRecipe(
            Eln.instance.veryHighVoltageCableDescriptor.newItemStack(12), "RRR", "CCC", "RRR", 'C', "ingotAlloy", 'R',
            "itemRubber"
        )
    }

    private fun recipeThermalCable() {
        addRecipe(
            Eln.findItemStack("Copper Thermal Cable", 12), "SSS", "CCC", "SSS", 'S',
            ItemStack(Blocks.cobblestone), 'C', "ingotCopper"
        )
        addRecipe(
            Eln.findItemStack("Copper Thermal Cable", 1), "S", "C", 'S', ItemStack(Blocks.cobblestone), 'C',
            findItemStack("Copper Cable")
        )
    }

    private fun recipeLampSocket() {
        addRecipe(
            Eln.findItemStack("Lamp Socket A", 3), "G ", "IG", "G ", 'G', ItemStack(Blocks.glass_pane), 'I',
            findItemStack("Iron Cable")
        )
        addRecipe(
            Eln.findItemStack("Lamp Socket B Projector", 3), " G", "GI", " G", 'G',
            ItemStack(Blocks.glass_pane), 'I', ItemStack(Items.iron_ingot)
        )
        addRecipe(
            Eln.findItemStack("Street Light", 1), "G", "I", "I", 'G', ItemStack(Blocks.glass_pane), 'I',
            ItemStack(Items.iron_ingot)
        )
        addRecipe(
            Eln.findItemStack("Robust Lamp Socket", 3), "GIG", 'G', ItemStack(Blocks.glass_pane), 'I',
            ItemStack(Items.iron_ingot)
        )
        addRecipe(
            Eln.findItemStack("Flat Lamp Socket", 3), "IGI", 'G', ItemStack(Blocks.glass_pane), 'I',
            findItemStack("Iron Cable")
        )
        addRecipe(
            Eln.findItemStack("Simple Lamp Socket", 3), " I ", "GGG", 'G', ItemStack(Blocks.glass_pane), 'I',
            ItemStack(Items.iron_ingot)
        )
        addRecipe(
            Eln.findItemStack("Fluorescent Lamp Socket", 3), " I ", "G G", 'G', findItemStack("Iron Cable"), 'I',
            ItemStack(Items.iron_ingot)
        )
        addRecipe(
            Eln.findItemStack("Suspended Lamp Socket", 2), "I", "G", 'G', findItemStack("Robust Lamp Socket"), 'I',
            findItemStack("Iron Cable")
        )
        addRecipe(
            Eln.findItemStack("Long Suspended Lamp Socket", 2), "I", "I", "G", 'G', findItemStack(
                "Robust Lamp " +
                        "Socket"
            ), 'I', findItemStack("Iron Cable")
        )
        addRecipe(
            Eln.findItemStack("Suspended Lamp Socket (No Swing)", 4), "I", "G", 'G', findItemStack(
                "Robust Lamp " +
                        "Socket"
            ), 'I', ItemStack(Items.iron_ingot)
        )
        addRecipe(
            Eln.findItemStack("Long Suspended Lamp Socket (No Swing)", 4), "I", "I", "G", 'G', findItemStack(
                "Robust Lamp Socket"
            ), 'I', ItemStack(Items.iron_ingot)
        )
        addRecipe(
            Eln.findItemStack("Sconce Lamp Socket", 2), "GCG", "GIG", 'G', ItemStack(Blocks.glass_pane), 'C',
            "dustCoal", 'I', ItemStack(Items.iron_ingot)
        )
        addRecipe(
            findItemStack("50V Emergency Lamp"), "cbc", " l ", " g ", 'c', findItemStack("Low Voltage Cable"),
            'b', findItemStack("Portable Battery Pack"), 'l', findItemStack("50V LED Bulb"), 'g',
            ItemStack(Blocks.glass_pane)
        )
        addRecipe(
            findItemStack("200V Emergency Lamp"), "cbc", " l ", " g ", 'c', findItemStack(
                "Medium Voltage " +
                        "Cable"
            ), 'b', findItemStack("Portable Battery Pack"), 'l', findItemStack("200V LED Bulb"), 'g',
            ItemStack(Blocks.glass_pane)
        )
    }

    private fun recipeLampSupply() {
        addRecipe(
            Eln.findItemStack("Lamp Supply", 1), " I ", "ICI", " I ", 'C', "ingotCopper", 'I',
            ItemStack(Items.iron_ingot)
        )
    }

    private fun recipePowerSocket() {
        addRecipe(
            Eln.findItemStack("Type J Socket", 16), "RUR", "ACA", 'R', "itemRubber", 'U', findItemStack(
                "Copper " +
                        "Plate"
            ), 'A', findItemStack("Alloy Plate"), 'C', findItemStack("Low Voltage Cable")
        )
        addRecipe(
            Eln.findItemStack("Type E Socket", 16), "RUR", "ACA", 'R', "itemRubber", 'U', findItemStack(
                "Copper" +
                        " Plate"
            ), 'A', findItemStack("Alloy Plate"), 'C', findItemStack("Medium Voltage Cable")
        )
    }

    private fun recipePassiveComponent() {
        addRecipe(
            Eln.findItemStack("Signal Diode", 4), " RB", " IR", " RB", 'R', ItemStack(Items.redstone), 'I',
            findItemStack("Iron Cable"), 'B', "itemRubber"
        )
        addRecipe(
            Eln.findItemStack("10A Diode", 3), " RB", "IIR", " RB", 'R', ItemStack(Items.redstone), 'I',
            findItemStack("Iron Cable"), 'B', "itemRubber"
        )
        addRecipe(findItemStack("25A Diode"), "D", "D", "D", 'D', findItemStack("10A Diode"))
        addRecipe(
            findItemStack("Power Capacitor"), "cPc", "III", 'I', ItemStack(Items.iron_ingot), 'c',
            findItemStack("Iron Cable"), 'P', "plateIron"
        )
        addRecipe(
            findItemStack("Power Inductor"), "   ", "cIc", "   ", 'I', ItemStack(Items.iron_ingot), 'c',
            findItemStack("Copper Cable")
        )
        addRecipe(
            findItemStack("Power Resistor"), "   ", "cCc", "   ", 'c', findItemStack("Copper Cable"), 'C',
            findItemStack("Coal Dust")
        )
        addRecipe(
            findItemStack("Thermistor"), "   ", "cCc", "   ", 'c', findItemStack("Copper Cable"), 'C',
            findItemStack("Copper Ingot")
        )
        addRecipe(
            findItemStack("Rheostat"), " R ", " MS", "cmc", 'R', findItemStack("Power Resistor"), 'c',
            findItemStack("Copper Cable"), 'm', findItemStack("Machine Block"), 'M', findItemStack("Electrical Motor"),
            'S', findItemStack("Signal Cable")
        )
        addRecipe(
            findItemStack("NTC Thermistor"), "   ", "csc", "   ", 's', "dustSilicon", 'c', findItemStack(
                "Copper Cable"
            )
        )
        addRecipe(
            findItemStack("Large Rheostat"), "   ", " D ", "CRC", 'R', findItemStack("Rheostat"), 'C',
            findItemStack("Copper Thermal Cable"), 'D', findItemStack("Small Passive Thermal Dissipator")
        )
    }

    private fun recipeSwitch() {
        addRecipe(
            findItemStack("Low Voltage Switch"), "  I", " I ", "CAC", 'R', ItemStack(Items.redstone), 'A',
            "itemRubber", 'I', findItemStack("Copper Cable"), 'C', findItemStack("Low Voltage Cable")
        )
        addRecipe(
            findItemStack("Medium Voltage Switch"), "  I", "AIA", "CAC", 'R', ItemStack(Items.redstone),
            'A', "itemRubber", 'I', findItemStack("Copper Cable"), 'C', findItemStack("Medium Voltage Cable")
        )
        addRecipe(
            findItemStack("High Voltage Switch"), "AAI", "AIA", "CAC", 'R', ItemStack(Items.redstone), 'A',
            "itemRubber", 'I', findItemStack("Copper Cable"), 'C', findItemStack("High Voltage Cable")
        )
        addRecipe(
            findItemStack("Very High Voltage Switch"), "AAI", "AIA", "CAC", 'R', ItemStack(Items.redstone),
            'A', "itemRubber", 'I', findItemStack("Copper Cable"), 'C', findItemStack("Very High Voltage Cable")
        )
    }

    private fun recipeElectricalRelay() {
        addRecipe(
            findItemStack("Low Voltage Relay"), "GGG", "OIO", "CRC", 'R', ItemStack(Items.redstone), 'O',
            findItemStack("Iron Cable"), 'G', ItemStack(Blocks.glass_pane), 'A', "itemRubber", 'I', findItemStack(
                "Copper Cable"
            ), 'C', findItemStack("Low Voltage Cable")
        )
        addRecipe(
            findItemStack("Medium Voltage Relay"), "GGG", "OIO", "CRC", 'R', ItemStack(Items.redstone), 'O',
            findItemStack("Iron Cable"), 'G', ItemStack(Blocks.glass_pane), 'A', "itemRubber", 'I', findItemStack(
                "Copper Cable"
            ), 'C', findItemStack("Medium Voltage Cable")
        )
        addRecipe(
            findItemStack("High Voltage Relay"), "GGG", "OIO", "CRC", 'R', ItemStack(Items.redstone), 'O',
            findItemStack("Iron Cable"), 'G', ItemStack(Blocks.glass_pane), 'A', "itemRubber", 'I', findItemStack(
                "Copper Cable"
            ), 'C', findItemStack("High Voltage Cable")
        )
        addRecipe(
            findItemStack("Very High Voltage Relay"), "GGG", "OIO", "CRC", 'R', ItemStack(Items.redstone),
            'O', findItemStack("Iron Cable"), 'G', ItemStack(Blocks.glass_pane), 'A', "itemRubber", 'I',
            findItemStack("Copper Cable"), 'C', findItemStack("Very High Voltage Cable")
        )
        addRecipe(
            findItemStack("Signal Relay"), "GGG", "OIO", "CRC", 'R', ItemStack(Items.redstone), 'O',
            findItemStack("Iron Cable"), 'G', ItemStack(Blocks.glass_pane), 'I', findItemStack("Copper Cable"), 'C',
            findItemStack("Signal Cable")
        )
        addRecipe(
            findItemStack("Low Current Relay"), "GGG", "OIO", "CRC", 'R', ItemStack(Items.redstone), 'O',
            findItemStack("Iron Cable"), 'G', ItemStack(Blocks.glass_pane), 'A', "itemRubber", 'I', findItemStack(
                "Copper Cable"
            ), 'C', findItemStack("Low Current Cable")
        )
        addRecipe(
            findItemStack("Medium Current Relay"), "GGG", "OIO", "CRC", 'R', ItemStack(Items.redstone), 'O',
            findItemStack("Iron Cable"), 'G', ItemStack(Blocks.glass_pane), 'A', "itemRubber", 'I', findItemStack(
                "Copper Cable"
            ), 'C', findItemStack("Medium Current Cable")
        )
        addRecipe(
            findItemStack("High Current Relay"), "GGG", "OIO", "CRC", 'R', ItemStack(Items.redstone), 'O',
            findItemStack("Iron Cable"), 'G', ItemStack(Blocks.glass_pane), 'A', "itemRubber", 'I', findItemStack(
                "Copper Cable"
            ), 'C', findItemStack("High Current Cable")
        )
    }

    private fun recipeWirelessSignal() {
        addRecipe(
            findItemStack("Wireless Signal Transmitter"), " S ", " R ", "ICI", 'R',
            ItemStack(Items.redstone), 'I', findItemStack("Iron Cable"), 'C', Eln.dictCheapChip, 'S', findItemStack(
                "Signal Antenna"
            )
        )
        addRecipe(
            findItemStack("Wireless Signal Repeater"), "S S", "R R", "ICI", 'R', ItemStack(Items.redstone),
            'I', findItemStack("Iron Cable"), 'C', Eln.dictCheapChip, 'S', findItemStack("Signal Antenna")
        )
        addRecipe(
            findItemStack("Wireless Signal Receiver"), " S ", "ICI", 'R', ItemStack(Items.redstone), 'I',
            findItemStack("Iron Cable"), 'C', Eln.dictCheapChip, 'S', findItemStack("Signal Antenna")
        )
    }

    private fun recipeChips() {
        addRecipe(
            findItemStack("NOT Chip"), "   ", "cCr", "   ", 'C', Eln.dictCheapChip, 'r',
            ItemStack(Items.redstone), 'c', findItemStack("Copper Cable")
        )
        addRecipe(
            findItemStack("AND Chip"), " c ", "cCc", " c ", 'C', Eln.dictCheapChip, 'c', findItemStack(
                "Copper " +
                        "Cable"
            )
        )
        addRecipe(
            findItemStack("NAND Chip"), " c ", "cCr", " c ", 'C', Eln.dictCheapChip, 'r',
            ItemStack(Items.redstone), 'c', findItemStack("Copper Cable")
        )
        addRecipe(
            findItemStack("OR Chip"), " r ", "rCr", " r ", 'C', Eln.dictCheapChip, 'r',
            ItemStack(Items.redstone)
        )
        addRecipe(
            findItemStack("NOR Chip"), " r ", "rCc", " r ", 'C', Eln.dictCheapChip, 'r',
            ItemStack(Items.redstone), 'c', findItemStack("Copper Cable")
        )
        addRecipe(
            findItemStack("XOR Chip"), " rr", "rCr", " rr", 'C', Eln.dictCheapChip, 'r',
            ItemStack(Items.redstone)
        )
        addRecipe(
            findItemStack("XNOR Chip"), " rr", "rCc", " rr", 'C', Eln.dictCheapChip, 'r',
            ItemStack(Items.r
