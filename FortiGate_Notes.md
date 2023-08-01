# FortiGate Notes

## Register FortiGate (FGT) with FortiManager (FMG)
Log into FGT (locally or remotely) as `admin`.

```
# config system global
	  set hostname NEW_HOSTNAME
  end

# config system central-management
      set type fortimanager
	  set fmg "123.123.123.123"   # IP address of FMG
  end
  
# exec central-management register-device FMG_SERIAL_NUMBER ""
```

Reboot FGT.
```
# exec reboot
```

Log into **FMG**. The FGT will be listed in the `root` ADOM as a **managed** device.