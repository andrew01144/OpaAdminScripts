# OpaAdminScripts
Some scripts to help with managing an Omni-Path fabric.

These are available with absolutely no support, and are in various states of completeness and functionality.
## Exploring the fabric
```
opaswitches <all|int|ext>
  List switches in the fabric.
  all - list guid and nodeDesc of all switches.
  int - list IP Addr and nodeDesc of all internally managed switches.
  ext - list guid and nodeDesc of all externally managed switches.
  Use instead of opagenswitches, opagenchassis. More convenient, less dependency.
   
opaswitchquery [-N guid1,guid2] [-L nodefile] [hwCheck|config|vpd]
  Report state and config of externally managed switches.
  Use instead of opaswitchadmin info|getconfig|hwvpd. More convenient report format.

opaextractswitchports
  List all switch ports, both online and offline.

opaextractbadports
  List all the ports in the fabric that appear to be a bad state.
  It will find ports with cables but no link, ports in in Training, etc.
  
opalinks
  Like opaextractsellinks, but generates two lines for each link, one in each orientation.
  This can be more convenient for post-processing than opaextractsellinks.

opalinkinfo
  Attempts to replicate the report format of iblinkinfo. Work in progress.

opaextractcablelinks
  Get a list of fabric links based on matching up cable serial numbers.
  Similar to opaextractsellinks, but does not require link to be up.
  May be confused by Y-Cables.

opaerrs
  List error counter values.
  Use instead of opareport -o errors -o slowlinks. More convenient report format.

opaPortBounce switch.port
  Generate the command to bounce a port. Copy and paste the command to execute it.
  Example:
  opaPortBounce em-edge04.3
  opaportconfig -l 0x001c -m 3 bounce     # em-edge04.3 current state: LinkUp/Active, 25Gb/4X/4X
```
## Checking topology
```
opaextractsellinks | topoDirectorCheck [--nSlinks <N>]
  Check all internal links of all director switches. Use --nSlinks 24 for directors with 48-port leafs.
  Use instead of opareport -o verifylinks -T linksum_swd24.xml. More convenient report format; requires no topology file.
  
topoDiff [-ci1h] <topofile> <fabricfile>
  Check that fabric links match the design file.
  Use instead opareport -o verifylinks -T topofile.xml. More convenient report format.
  
topoMatchSwitches
  Determine the name (NodeDesc) of each edge switch by matching to the topology file.
  For 3-tier Direct/Edge fabrics only.
  Use instead of opagenswitches. More convenient, does not depend on correctly named compute nodes.
  
topoApplyGuids2Desc
  Updates the NodeDesc in an opaextractsellinks file according to a GUID:NodeDesc file.
  Allows you to check topology before the NodeDesc in the switches has been configured.
  
topoUnApplyGuids2Desc
  Reverses topoApplyGuids2Desc. Useful for testing.
```

## Other stuff

```
orientLinks               - used by other scripts.
topoDirectorGen           - for testing?
extractFromSnapshot       - simulate opaextract{sellinks,lids},opafabricinfo on a standlone machine. Currently broken?

opaSwitchQueryFanSpeeds   - get fan speeds from ext-managed switches
opaerrs_April2016         - earlier version of opaerrs
topoDiffBasic             - demo of a simple script
topoDirectorCheckSimple   - demo of a simple script
  
topo_txt2xml              - Converts a simple Name1;Port1;Name2:Port2 text file to xml for use in opareport -o verifylinks.
  
export PATH=/install/AndrewRussell/OpaAdminScripts/bin:$PATH
```



