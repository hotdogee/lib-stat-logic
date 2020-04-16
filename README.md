# LibStatLogic

A World of Warcraft addon library for stat conversion, calculation and summarization.

# Features

* StatConversion
  * Ratings -> Effect
  * Str -> AP, Block
  * Agi -> Crit, Dodge, AP, RAP, Armor
  * Sta -> Health, SpellDmg(Talent)
  * Int -> SpellCrit
  * Spi -> MP5
  * and more!
* StatMods - Get stat mods from talents and buffs for every class
* BaseStats - for all classes and levels
* ItemStatParser - Fast multi level indexing algorithm instead of calling strfind for every stat

# Slash Commaands

* /sldebug

* /slwarning
  * Allows libstatlogic to show warnings if an item is not recognized.
  * This is a compromise between nothing and full /sldebug mode

# About ratings systems

World of Warcraft has values called "Combat Ratings". This value is a pure number. 

For example, a piece of armor might have "+1,069 Mastery" on it. 
You can get the player's current Combat Rating for Mastery by calling the Blizzard API function:

    GetCombatRating(CR_MASTERY); --e.g. returns 8833

Combat Ratings then give the player a corresponding "Combat Rating Bonus". 
This bonus is generally given as percentage. E.g.:

    Haste Rating   of 10,784 gives +25.37% Haste
    Mastery Rating of  8,833 gives +14.72% Mastery
    Hit Rating of      2,688 gives  +7.91% Hit Chance

You can get the player's Combat Rating Bonus for Mastery by calling the Blizzard API function:

    GetCombatRatingBonus(CR_MASTERY); --e.g. returns 14.72166633606 (which means +14.72% Mastery)
    
So it's important to know that the player has two different combat statistics:
    - combat rating: GetCombatRating(combatRatingID);
    - combat rating bonus: GetCombatRatingBonus(combatRatingID);

LibStatLogic has its own GetCombatRatingBonus, that attempts to calculate the secret undocumented
conversions of Ratings to Bonuses:

    /dump StatLogic:GetEffectFromRating(1500, CR_PVP_POWER)

LibStatLogic has to maintain separate values for your Rating and RatingBonus. 
That is why LibStatLogic uses different identifiers than just CR_xxx: