# Cold-Silicon

## Genral Guidelines:
- All passive components (capacitors, resistors, inductors) should be placed in the "bschulz_passives" library to avoid cluttering the main library with excessive packages
- If a general part already exists, add your part as a subset of that, for example, if you want to add an LED, simply add another pinout/technology to the already existing LED device
- Fully document the part using technologies whenever possible! (See use of technologies below)

## Design Guidelines:

### Layer Usage:
- Use `tPlace` for part outlines
- Use `tNames` for part names (Usually will just be a `>NAME` label)
- Use `tValues` for part values (Either a hard coded name like "LM1117", or `>VALUE` label)

### Text:
Text in parts should use the following:
- Size = 0.6096mm = 24mil = 0.024in 
- Ratio = 15%
- Line Distance = 50%
- Font = Vector
- Align = Center

If you find otherwise, it is because I am indeed a flawed human being! 

## Naming Conventions:
Connectors: CONN_[NumberOfPinsPerRow]X[NumberOfRows]
- Adding number of rows is optional, if only one row can ommit X and the NumberOfRows

## Technologies:
Technologies combined with attributes are the greatest friend you never knew you had! This library attempts to use technologies and attributes to a large extent to reduce human error and simply make things more efficient. 

In this library technologies are used in two ways:
1. To denote a subset of a part
2. To denote a specific value of a genaric parts

**Example of case 1:**
You are want to add a TPS797 voltage regulator, you find that this part comes in several different voltage options which are denoted by different part numbers, these include 1.8v, 2.85v, and 3.3v, which have the part numbers TPS79718, TPS797285, TPS79733. You will quickly notice that TPS797 is the general part number, followed by sub number which specifies the operation of the part. In this case when you create the device you would name it `TPS797*`, Eagle allows you to place an asterisk in the part name, which will be substituted with the specific technology when selected. In this case you would make the technologies 18, 285, and 33 (See below on the details of how to add a technology). Now populate each of these technologes with the appropriate attributes. Now when you go to access your library, you will see your part device name (with the asterisk) and a drop down menu, when you click on the dropdown you will find TPS79718, TPS797285, TPS79733, where the asterisk has been replaced by the technology name, how fast was that! Now when you select, say, TPS79733 from this list and place it on your board, it already has the specific attributes for that part assigned to it, viola!

**Example of case 2:**
This case is more simple than case 1, and usually applies to passive components. In this case you have a general part, say, an 0603 capacitor. If you go into the capacitor device, and then click on the appropriate package variant, you can add a technology which specifies the value of the device. Since there is no asterisk in the part name, Eagle will not create a sub part for each technology in the dropdown menu (which is good, beacuse can you imagine how big that could get if you have a dozen different 0603 capacitors you use?! Also, this allows you to place the part as a genaric component, then change the technology later, as you figure out the exact values you want in your design). In this case you can simply add a technology which is named by the value or special features of the part. So for our capacitor we may want to add a 1uF capacitor, in which case we would call the technology `1UF`. If we have issues where we have several 1uF capacitors we use all the time, which have say different voltage ratings or tolerances, then we can call our techology `1UF,16V` or something along those lines. It should be noted this is not how you add **every** passive component, this is only done for parts which are used a lot, and as a result conflicts like this are not very common, at least how this library gets used. If we need to use a special part on some board, we will just enter the attributes manually. Once you add this technology (along with the appropriate attributes), you can place the genaric part in your schematic, then change the technology by clicking on the wrench in the upper left of the schematic toolbar, then click "technology", now click on the part and you will see a list of available technologies pop up, simply click on the one you want to use for that part.

Following these technology and attribute guidelines greatly reduce the number of part numbers which need to be attributed manually, which makes things like BOM generation become exceedingly easier, because you do not have to manually add the part numbers each time (which also reduces the likelyhood you will do it wrong, since it only needs to be done once!)

#### How to add a technologie(s)/attributes
When creating a device, you will see an option for technologies, if you click this it will open up a window which will allow you to add new technologies (either by clicking on the boxes of existing ones, or by typing them in, seperated by spaces, below). Then click ok, you will now see your technologies table has expanded! Click on the word "attributes", if there are no attributes you will just see a list of technologies on the left with blank space to the right. Otherwise you might see a list of attributes "DIGIKEY, MF, MPN" with some of them populated. If this is the case, click on the empty cells, and click "change" to moddify that value for that technology. If the columns to the right are blank, then you will want to click on the technology row you want to edit, then say "new" and type in the name of the technology to add (see below) in the "Name" box, then type in the appropriate value in the "Value" box. 

Protip: If you want to add a value which is the same for all of the technologies (say the manufacturer), you can select "all" from the dropdown menu at the bottom of this dialog box

#### How to add attributes manually
If you do not want to add the attributes/technologies to the parts themselves for whatever reason (which there are plenty of valid ones), you can simply add these attributes to individual parts by clicking on the "Attribute" button in the lower left of the schematic menu, then click on an individual part and enter these attributes in the same manor you would for adding them to a technology. 

The following attributes should be present in all parts whenever possible:
- `DIGIKEY`, This is the digikey PN
- `MPN`, this is the manufacturers part number (sometimes this is the same, if so, just put the same value in both entries)
- `MF`, this is the manufacturer, which is sometimes abbreviated (see list below), this is less important, but I think it is helpful none the less



Manufacturer List:
Name(AttributeName)

Linear Technology (LT)
Texas Insturments (TI)
Murata (Murata)
Wurth (Wurth)
Cree (Cree)
Amphenol (Amphenol)
Molex (Molex)
3M (3M)
Panasonic (Panasonic)
UBlox (UBlox)
Vishay (Vishay)
Luna Optoelectronics (Luna Opto)
Marktech Optoelectronics (Marktech)

### Part Descriptions:
Part descriptions should utilize the ability to use HTML formatting in the description text and include the name and type of device, along with links to the part (or parts) on Digikey. The link text should consists of the Digikey part number, or the Part Number (if there are multiple versions of the part), or both. An example descripton is given below

```
<b>TPS6306x</b> - Step Up/Down Converter (Buck-Boost)
 
<p>Characteristics:
<ul>
<li>Vin: 2.5V ~ 12V</li>
<li>Vout: 2.5V ~ 8V (or 5V fixed)</il>
<li> Output Current: 2 A </li>
<li>Operating Temperature: -40°C to 85°C</li>
</ul>
</p>
 
<p>Digikey: <br>
<ul>
<a href = "https://www.digikey.com/product-detail/en/texas-instruments/TPS63061DSCR/296-30205-1-ND/2834997"> 296-30205-1-ND, TPS63061 (5V Fixed) </a><br/>
<a href = "https://www.digikey.com/product-detail/en/texas-instruments/TPS63060DSCR/296-30204-1-ND/2834996"> 296-30204-1-ND, TPS63060 (Adjustable) </a> 
</ul>
</p>
```

### Included Parts:
