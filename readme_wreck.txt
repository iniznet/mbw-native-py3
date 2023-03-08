-------------------
| Compatibilities |
-------------------

1) WRECK works on Python version 2.6.x or 2.7.x! Doesn't work with 2.5.x
2) [WRECK integrated] Doesn't support ModMerger Framework and KT0 autoresolver script. Use WRECK plugin system instead.
3) Global variables in quotes are not case sensitive (unlike vanilla compiler "built_module.bat") - "$g_My_Variable" and "$g_my_variable" are the same variable in WRECK but Vanilla compiler thinks they are different. Advanced expressions should be all lower case.
4) WRECK doesn't track usage of global variables. Native compiler add +1 to entry in the file "variable_uses.txt". So the warning "Global variable declared but never used" is shown only when you add new global variable but not when you remove code where it was used. You need to delete file "variable_uses.txt" to see this warning. This file together with "variables.txt" is used to create reserved global variables for compatibility with new updates. For this you need to add reserved global variable to file "variables.txt" and add 1 to the same line in the file "variable_uses.txt". This is redundant work. Also why you need reserved order of identificators for global variables? There is no operators like try_for_global_variables. Use slots of troops instead.
5) [WRECK integrated] Unlike vanilla it was not possible to copy with python entities (party, item, troop, etc) because they are complex recursive wreck objects. Example - generattion of no-swing weapons in Motomataru formation script. For this use new function <newcopy()> it was added in v1.0.3.
6) Viking Conquest module system adds trigger to all mission templates automatically. So you need to add code for WRECK in the end of "module_mission_templates.py".
CODE:
-----------------------------------------------------------------
import __main__
if __main__.__file__ == 'compile.py':#This is WRECK compiler?
   for mission in mission_templates:
      mission[5].append(*global_common_triggers)
-----------------------------------------------------------------


--------------
| Change log |
--------------

v1.0.6
1) Improved compatibility with vanilla compiler. WRECK produces 99% of vanilla code. Exceptions are mission template triggers. They have counters (trigger_ID) in the last number of fake (useless) operand. Items don't generate hit_point values if they don't have them.
2) Identifiers and global variables decapitilised and white spaces changed to underscore. WRECK shows notices about capital characters. You need to change them for compatibility with vanilla compiler wich not always "cure" identifiers. To disable decapitalisation add "cap" to command line. Only music tracks are exception for this rule.
3) Faction relations use the last relation value in module like vanilla. WRECK shows conflicts to resolve. To reduce notices add "fac" to command line.

Keep in mind that "cap" and "fac" options are only temporary. Better to fix all notices for compatibility with vanilla compiler and reducing bugs.
compile.bat CODE:
---------------------------------------------------------
python compile.py tag cap fac %1 %2 %3 %4 %5 %6 %7 %8 %9
---------------------------------------------------------



v1.0.5
1) Added more detailed explanation to hint of syntax errors.

v1.0.4
1) In mission templates added unused operand to the end of each trigger: <(lt, -1, id),>
where ID is the number of this trigger starting from 0. Helper for debugging engine error messages.
2) Removed duplicate notices about unused local variable of repeatable mission triggers.
3) When compiler message show trigger name, it will be more informative - with each of the three timers if trigger is not simple. Previously was only the first.

v1.0.3
1) [WRECK integrated] fixed expressions in module_mission_template.py triggers. They were bugged if used more than once.
2) Added checking audio file extensions.
3) WRECK will check output dialog states like in Native compiler.

v1.0.2
1) Script that can fail will be checked for letters "cf_" in the begining of its name.
2) Local variables that have "unused" in their name will not be checked for warning "Variable declared but never used".
3) [WRECK integrated] Warning "Variable declared but never used" will now check expressions.
4) [WRECK integrated] Local variables declared by WRECK expressions in the loop conditions (try_for_range) will not be overriden by temp variable inside loop body.

v1.0.1
1) Fixed processing module_skins.py - removed extra spaces wich led to errors in OpenBrf
2) Fixed processing module_sounds.py - sound file can now be declared as list with faction ["sound_file.flac", fac_outlaw]. WRECK works with WithFire&Sword and NapoleonicWars MS now.
3) try_for_range_backwards will not show unused variable warning.