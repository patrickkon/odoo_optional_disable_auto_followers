# odoo_optional_disable_auto_followers
Allow optional disable automatic followers (for all modules) functionality in base.config.settings

# Note: 
1. This overrides corresponding files in base mail module.
2. Works for Odoo V8.0.

The fix is an enhanced/simplified version of this module: https://apps.odoo.com/apps/modules/8.0/sale_disable_auto_followers/  
  
# How to use?
1. Simply download and install the addon into your odoo instance (make sure that it overrides base mail module in the install process).
2. Toggle “Disable Followers Functionality” in General Settings.   
3. Play with modules like sale.order that originally have followers functionality.  

# Descriptions/ explanations:
  
## For disabling automatic follower functionality:  
  
#### Approaches considered that do not work:  
  
##### Extend mail.thread model to override required functions (i.e. inherited mail.thread → name mail.thread)  
  
The functions are/might not be called.  
  
For example:  
  
1. This is the intended effect: sale.order → my inherited mail.thread → name mail.thread  
  
2. However my inherited mail.thread is not called.  
  
3. The actual chain is:  
  
   a. sale.order → name mail.thread  
  
   b. my inherited mail.thread → name mail.thread  
  
4. Workaround that is not recommended but can work:  
  
   a. add depends “my inherited mail.thread module” in manifest file of product module (since product inherits directly from mail module)  
  
5. Note: although point 4 works because of our knowledge that function calls are based on module loading order (“depends” key), it is actually a bug that is fixed after odoo v8.  
  
    a. Meaning, model extensions should actually be effective in children models.  
  
      i. In our example, the model extension is “my inherited mail.thread”. The child model is the child of “name mail.thread”, which is “sale.order”.  
  
    b. Refer to:  
  
      i. https://stackoverflow.com/questions/31936122/how-to-inherit-mail-thread-abstractmodel-and-override-function-from-this-class-i  
  
      ii. https://github.com/odoo/odoo/issues/9084  
      
  
#### Approaches considered that are not effective:  
  
1. Use the given module above. Replicate for other affected modules 1 by 1.  
  
   
  
#### Current approach (it is the most simplified):  
  
1. Copy related mail module files.  
  
2. Add flag in ir.config_parameter  
  
3. If flag true, disable followers functionality in related mail module functions.  
  
   
  
## For allowing manual follower functionality:  
  
Simply pass a context key to the only 2 locations where a user can manually add followers: “Follow” and “Add Others” buttons.  
