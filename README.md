# OnePlus Dolby
## Getting Started
For dolby media codecs to work, add this line in your media codecs xml (should be in vendor partition) :-
```
<Include href="media_codecs_dolby_audio.xml" />
```
Add the dolby effects in your device's audio_effects.xml :-
```
<!--DOLBY DAP-->
<library name="dap_sw" path="libswdap.so"/>
<library name="dap_hw" path="libhwdap.so"/>
<!--DOLBY END-->
<!--DOLBY GAME-->
<library name="gamedap" path="libswgamedap.so"/>
<!--DOLBY END-->
```
```
<!--DOLBY DAP-->
<effectProxy name="dap" library="proxy" uuid="9d4921da-8225-4f29-aefa-39537a04bcaa">
    <libsw library="dap_sw" uuid="6ab06da4-c516-4611-8166-452799218539"/>
    <libhw library="dap_hw" uuid="a0c30891-8246-4aef-b8ad-d53e26da0253"/>
</effectProxy>
<!--DOLBY END-->
<!--DOLBY VQE-->
<effect name="vqe" library="vqe" uuid="64a0f614-7fa4-48b8-b081-d59dc954616f"/>
<!--DOLBY END-->
```
Inherit the dolby makefile by adding this in your device's makefile (device.mk) :-
```makefile
$(call inherit-product, hardware/dolby/dolby.mk)
```
Add this line in your BoardConfig.mk in device tree :-
```
AUDIO_FEATURE_ENABLED_HW_ACCELERATED_EFFECTS := true
```
Remove these properties from vendor.prop in device tree :-
```
vendor.audio.dolby.ds2.enabled=false
vendor.audio.dolby.ds2.hardbypass=false
```
Add this hal in your device's hidl framework compatibility matrix xml :-
```
<hal format="hidl" optional="true">
    <name>vendor.dolby.hardware.dms</name>
    <version>2.0</version>
    <interface>
        <name>IDms</name>
        <instance>default</instance>
    </interface>
</hal>
```
Add this hal in your device's hidl manifest xml :-
```
<hal format="hidl">
    <name>vendor.dolby.hardware.dms</name>
    <transport>hwbinder</transport>
    <fqname>@2.0::IDms/default</fqname>
</hal>
```
At the end an example commit to properly implement it in your device tree could be :-
Device Tree :- https://github.com/narikootam-dev/device_xiaomi_sweet/commit/b36211a352897afedd35feabffa12af54f21716c
