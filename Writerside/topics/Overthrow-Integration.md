# Overthrow Integration

If your goal is to get prefabs working in both our mod (spawning in containers), and working with Overthrow's shop system. This is the guide!

Our looting mod and Overthrow do things a bit different. We both scrape the **SCR_FactionManager** for faction items! However, from there we diverge.

TrainWreckLooting exposes a json file that you can tweak as a server-owner - so you can tailor it to your liking. 
Overthrow uses `.conf` files that are only accessible from the Workbench

## Pricing

If you add a new category, overthrow needs to be told what to do with said category. If it comes across a category it doesn't know about, it 
gets skipped. You will want to override the **itemPrices.conf**.

![overthrow-pricing-conf.png](overthrow-pricing-conf.png)

Lets open up one of the entries. Note, the **Item Type** is **SNIPER_RIFLE**. Overthrow is leveraging the 
same **SCR_EArsenalItemType** - so your category should appear in the dropdown!

![overthrow-sniper-category.png](overthrow-sniper-category.png)

You may categorically prices things, and provide a demand OR you can get a bit granular and apply special demand/pricing 
for certain items.

If you look at the Morphine and Saline bag they are both in the same category but receive custom settings based on the **Find** field. 

Overthrow scans the resource name for that text and if it matches, the settings will be applied.

![overthrow-medicine.png](overthrow-medicine.png)

Notice, you can also specify items you do NOT want in the shop by checking the **hidden** field.
