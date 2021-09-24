# Table of content
  - [Network Diagram](#Network-Diagram)
  - [To configure the FortiManager HA](#To-configure-the-FortiManager)
  - [To configure the FortiGate HA](#To-configure-the-FortiGate)
  - [Upgrade the FortiManager from 6.0.11 GA to 6.4 GA](#Upgrade-the FortiManager-from-6.0.11-GA-to-6.4-GA)
  - [Conclusion)

# NetWork Digram

```
FortiManager( 10.210.34.68) --(FGT  10.210.34.67)

# To configure the FortiManager 

```
Firstly, FMG is in 6.0.11: v6.0.11-build0478 210608 (GA)

IP is : 10.210.34.68
config system interface
    edit "port1"
        set ip 10.210.34.68 255.255.254.0
        set allowaccess ping https ssh http
    next
end

config system route
    edit 1
        set device "port1"
        set gateway 10.210.35.254
    next
end

config system global
    set adom-status enable
    set hostname "test-fmg-60"
    set usg enable
end

2 Adoms:

-root
-6_0

```

# To configure the FortiGate 

```

config system global
    set alias "FGVMULTM21001410"
    set hostname "test-fos-60"
    set timezone 04
end

config system interface
    edit "port1"
        set vdom "root"
        set ip 10.210.34.67 255.255.254.0
        set allowaccess ping https ssh http fgfm
        set type physical
        set snmp-index 1
        next
   end
   
   config router static
    edit 1
        set gateway 10.210.35.254
        set device "port1"
    next
end
config system central-management
    set type fortimanager
    set fmg "10.210.34.68"
end

```

# Upgrade the FortiManager from 6.0.11 GA to 6.4 GA

```
Upgrade path to follow is 6.0.11 GA --> 6.2.8 GA --> 6.4.7 GA

Following : https://docs.fortinet.com/document/fortimanager/6.4.7/upgrade-guide/295114/fortimanager-firmware-upgrade-paths-and-supported-models
and  https://docs.fortinet.com/document/fortimanager/6.2.8/upgrade-guide/295114/fortimanager-firmware-upgrade-paths-and-supported-models

From FortiManager System Information GUI, upgrade firstlu 6.0.11 GA to 6.2.8 GA then to 6.4.7 GA


```

# Conclusion

```
