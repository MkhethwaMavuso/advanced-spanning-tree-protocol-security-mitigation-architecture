# advanced-spanning-tree-protocol-security-mitigation-architecture
Layer 2 Enterprise Campus Topology featuring multiple interconnected Cisco 2960 Catalyst switches (Switch0 with MAC 0090.2BC7.9AB4, Switch1 with MAC 0001.C76C.2853, Switch2, an old switch with explicit STP Priority 36864), and an unauthorized rogue switch (Bad guy configured with malicious STP Priority 0) attempting a Layer 2 topology takeover.

🌐 Quick Overview & Network Specs🟢 Architecture & Topology: Layer 2 Enterprise Campus Topology featuring multiple interconnected Cisco 2960 Catalyst switches (Switch0 with MAC 0090.2BC7.9AB4, Switch1 with MAC 0001.C76C.2853, Switch2, an old switch with explicit STP Priority 36864), and an unauthorized rogue switch (Bad guy configured with malicious STP Priority 0) attempting a Layer 2 topology takeover.   🔵 Endpoint & Subnet Allocations:PC0: IP 192.168.1.1 connected to access ports.   PC1: IP 192.168.1.2 connected to access ports.   🟣 Advanced STP Security Features & Directives: The lab focuses on hardening the Spanning Tree topology by deploying four critical defense mechanisms:   BPDU Guard: Prevents rogue switches (like the Bad guy switch with priority 0) from connecting to user access ports by shutting down ports upon receiving BPDUs.Root Guard: Protects the legitimate Root Bridge from being usurped by inferior or superior BPDUs originating from unauthorized downstream ports.BPDU Filter: Suppresses outgoing BPDUs or stops processing incoming BPDUs on specified edge ports.Loop Guard: Protects against STP alternate/root port failures transitioning into designated forwarding states due to unidirectional link failures.💡 Key Technical Insights & What This Lab Demonstrates🔍 STP Root Bridge Manipulation & Rogue Mitigation: Demonstrates how an unauthorized attacker (Bad guy switch with priority 0) attempts to manipulate bridge priorities to become the root bridge, intercepting all campus traffic, and how Root Guard and BPDU Guard neutralize this threat.   ⚡ Bridge ID & Priority Determinism: Explores how switches evaluate Bridge IDs (combining configurable STP priority values like 36864 or 0 with burned-in MAC addresses) to elect root and designated ports.🛠️ PortFast Integration with Security Controls: Outlines best practices for combining portfast access edge configuration with BPDU Guard to provide instant forwarding states for workstations while instantly isolating unauthorized switches.🛡️ Layer 2 Resiliency: Prevents bridging black holes, broadcast storms, and topology reconvergence delays caused by malfunctioning or hostile networking hardware.   🛠️ Essential Cisco CLI Configuration SnippetsEnabling BPDU Guard and Root Guard on Access/Trunk PerimetersCisco CLIinterface FastEthernet0/1
 description Access Port with BPDU Guard Enabled
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
!
interface GigabitEthernet0/1
 description Trunk Port with Root Guard Enabled
 spanning-tree guard root
Configuring Loop Guard and BPDU FilterCisco CLIinterface GigabitEthernet0/2
 spanning-tree guard loop
 spanning-tree bpdufilter enable
🧪 Verification & Audit CommandsBash# Verify STP root bridge details, local bridge ID, and designated bridge parameters
show spanning-tree root

# Inspect interface-level STP port states, roles, guard mechanisms, and BPDU counters
show spanning-tree interface fastEthernet 0/1 detail

# Check for ports placed into err-disabled state due to BPDU guard violations
show interfaces status err-disabled
