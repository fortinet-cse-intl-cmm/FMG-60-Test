# Table of content
  - [Network Diagram](#Network-Diagram)
  - [To configure the FortiManager HA](#To-configure-the-FortiManager)
  - [To configure the FortiGate HA](#To-configure-the-FortiGate)
  - [Upgrade the FortiManager from 6.0.11 GA to 6.4 GA](#Upgrade-the FortiManager-from-6.0.11-GA-to-6.4-GA)
  - [Conclusion)

# NetWork Digram

```
FortiManager( 10.210.34.68) --(FGT  10.210.34.67)

```
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
Upgrade path to follow is 6.0.11 GA --> 6.2.8 GA --> 6.4.7 GA -> 7.0.1 GA

Following : https://docs.fortinet.com/document/fortimanager/6.4.7/upgrade-guide/295114/fortimanager-firmware-upgrade-paths-and-supported-models
and  https://docs.fortinet.com/document/fortimanager/6.2.8/upgrade-guide/295114/fortimanager-firmware-upgrade-paths-and-supported-models
and

https://docs.fortinet.com/document/fortimanager/7.0.1/upgrade-guide/295114/fortimanager-firmware-upgrade-paths-and-supported-models

From FortiManager System Information GUI, upgrade firstlu 6.0.11 GA to 6.2.8 GA then to 6.4.7 GA and finally from 6.4.7 GA to 7.0.1 GA
```

# Conclusion


```
After the first upgrade, adom : 6_0 is conserved and root adom is also in 6.0 

After the second upgrade, same situation than after the first upgrade

After the third upgrade from6.4.7 GA to 7.0.1, adom 6_0 is version 6.0 is still visible and the registered 6.0 fgt is also available , see screenshots and this fmg output :

test-fmg-60 # diag dvm device list
--- There are currently 1 devices/vdoms managed ---
--- There are currently 0 devices/vdoms count for license ---

TYPE            OID    SN               HA      IP              NAME                                             ADOM                                             IPS                FIRMWARE
fmgfaz-managed  135    FGVMULTM21001410 -       10.210.34.67    test-fos-60                                      6_0                                              6.00741 (regular)  6.0 MR0 (443)
		|- STATUS: dev-db: not modified; conf: in sync; cond: OK; dm: autoupdated; conn: up; FMGC
		|- vdom:[3]root flags:0 adom:6_0 pkg:[never-installed]

--- There are currently 0 FortiAP managed ---


--- There are currently 0 FortiSwitch managed ---


--- There are currently 0 FortiExtender managed ---


--- End device list ---
```
