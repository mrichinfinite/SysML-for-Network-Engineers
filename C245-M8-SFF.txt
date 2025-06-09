classDiagram
    class ServerChassis {
        formFactor : String = 2RU
        part cpu[1..2] : Processor
        part dimm[0..24] : MemoryModule
        part drive[0..28] : StorageDevice
        part fan[6] : CoolingFan
        part psu[1..2] : PowerSupplyUnit
        part pcieSlot[0..8] : PCIeExpansionSlot
        part mLOM[0..1] : ModularLANonMotherboard
        part ocp[0..1] : OCP3_0Slot
        part storageController[0..2] : StorageController
        part led[*] : LEDIndicator
        part assetTag[1] : AssetTag
        part kvmConnector[1] : KVMConnector
        part usbPort[1] : USBPort
        part railKit[1] : RailKit
        reference bmc : BaseboardManagementController
        reference software : ManagementSoftware
    }
    class Processor {
        cores : Integer[1..160]
        generation : String = AMD EPYC 9005 Series
        cores : Integer[1..128]
        generation : String = AMD EPYC 9004 Series
        power : String = Up to 400W
        reference firmware[*] : Firmware
    }
    class MemoryModule {
        capacity : String = Up to 256 GB
        speed : String = Up to 6000 MT/s
        type : String = DDR5
    }
    class StorageDevice {
        type : String = SAS/SATA or NVMe
        size : String = 2.5-inch SFF
        location : String = Front bays 1-24 or Rear
        reference firmware[*] : Firmware
    }
    class CoolingFan {
        type : String = Hot-swappable
        reference firmware[*] : Firmware
    }
    class PowerSupplyUnit {
        power : String = 1050W, 1600W, or 2300W
        type : String = Hot-swappable
        reference firmware[*] : Firmware
    }
    class PCIeExpansionSlot {
        generation : String = Gen 4.0 or 5.0
        slots : Integer[0..8]
        reference nic[0..1] : NetworkInterfaceController
        reference storageController[0..1] : StorageController
        reference gpu[0..1] : GPUCard
    }
    class GPUCard {
        type : String = Double or single-wide
        power : String = Up to 300W
        location : String = Riser 1 Slot 2 or Riser 2 Slot 5
        model : String = H100, L40, L4, L40S, A16, MI210
        reference firmware[*] : Firmware
    }
    class ModularLANonMotherboard {
        connectivity : String = 10/25/50 Gbps or 40/100/200 Gbps
        reference vic[0..1] : VirtualInterfaceCard
    }
    class OCP3_0Slot {
        management : String = Out-of-band
        reference nic[0..1] : NetworkInterfaceController
        reference vic[0..1] : VirtualInterfaceCard
    }
    class StorageController {
        
        reference firmware[*] : Firmware
    }
    class TriModeRAID_24G {
        type : String = Cisco Tri-Mode 24G SAS RAID Controller
        cache : String = 4 GB FBWC
        part raid[1] : RAIDConfiguration
        part supercap[0..1] : SuperCapUnit
        reference storageDevice[0..24] : StorageDevice
    }
    class TriModeMP1_RAID {
        type : String = 24G Tri-Mode MP1 RAID Controller
        cache : String = 4 GB FBWC
        bracket : String = 2U
        part raid[1] : RAIDConfiguration
        part supercap[0..1] : SuperCapUnit
        reference storageDevice[0..32] : StorageDevice
    }
    class TriModeM1_HBA {
        type : String = 24G Tri-Mode M1 HBA
        capacity : String = Up to 16 drives per HBA
        reference storageDevice[0..16] : StorageDevice
    }
    class SAS_HBA_9500_8e {
        type : String = Cisco 12G 9500-8e SAS HBA
        function : String = External JBOD attach
        reference externalStorage[*] : ExternalStorageDevice
    }
    class RAIDConfiguration {
        type : String = Hardware RAID
    }
    class SuperCapUnit {
        function : String = Cache backup for power loss
        lifespan : String = Approx 3 years
    }
    class ExternalStorageDevice {
        type : String = JBOD
        interface : String = 12G SAS
        reference firmware[*] : Firmware
    }
    class NetworkInterfaceController {
        type : String = Standard NIC, 10/25/50 Gbps
        reference port[1..4] : PhysicalPort
        reference firmware[*] : Firmware
    }
    class VirtualInterfaceCard {
        type : String = Cisco UCS VIC 15000 Series
        ports : String = Quad-port 10/25/50 Gbps or Dual-port 40/100/200 Gbps
        reference port[1..4] : PhysicalPort
        reference firmware[*] : Firmware
    }
    class PhysicalPort {
        type : String = Network
        interface : String = SFP28, QSFP28, QSFP56, or QSFP-DD
        speed : String = 10/25/50/40/100/200 Gbps
    }
    class LEDIndicator {
        type : String = Status or Diagnostic
    }
    class AssetTag {
        identifier : String
    }
    class KVMConnector {
        function : String = Supplies video, keyboard, and mouse
    }
    class USBPort {
        version : String = USB 3.0 operates at 2.0 speed
    }
    class RailKit {
        type : String = Ball-bearing slide rail kit
    }
    class BaseboardManagementController {
        part firmware : Firmware
        reference serverOS : ServerOperatingSystem
        reference management : ManagementSoftware
    }
    class Firmware {
        version : String
    }
    class ServerOperatingSystem {
        part vm[0..n] : VirtualMachine
        reference firmware[*] : Firmware
    }
    class VirtualMachine {
        reference os : ServerOperatingSystem
    }
    class ManagementSoftware {
        type : String = Cisco Intersight, UCS Manager, or IMC
        deployment : String = SaaS, virtual appliance, or on-premise
    }

    ServerChassis *-- Processor
    ServerChassis *-- MemoryModule
    ServerChassis *-- StorageDevice
    ServerChassis *-- CoolingFan
    ServerChassis *-- PowerSupplyUnit
    ServerChassis *-- PCIeExpansionSlot
    ServerChassis *-- ModularLANonMotherboard
    ServerChassis *-- OCP3_0Slot
    ServerChassis *-- StorageController
    ServerChassis *-- LEDIndicator
    ServerChassis *-- AssetTag
    ServerChassis *-- KVMConnector
    ServerChassis *-- USBPort
    ServerChassis *-- RailKit
    ServerChassis o-- BaseboardManagementController
    ServerChassis o-- ManagementSoftware
    Processor o-- Firmware
    StorageDevice o-- Firmware
    CoolingFan o-- Firmware
    PowerSupplyUnit o-- Firmware
    PCIeExpansionSlot o-- NetworkInterfaceController
    PCIeExpansionSlot o-- StorageController
    PCIeExpansionSlot o-- GPUCard
    GPUCard o-- Firmware
    ModularLANonMotherboard o-- VirtualInterfaceCard
    OCP3_0Slot o-- NetworkInterfaceController
    OCP3_0Slot o-- VirtualInterfaceCard
    StorageController <|-- TriModeRAID_24G
    StorageController <|-- TriModeMP1_RAID
    StorageController <|-- TriModeM1_HBA
    StorageController <|-- SAS_HBA_9500_8e
    TriModeRAID_24G *-- RAIDConfiguration
    TriModeRAID_24G *-- SuperCapUnit
    TriModeRAID_24G o-- StorageDevice
    TriModeMP1_RAID *-- RAIDConfiguration
    TriModeMP1_RAID *-- SuperCapUnit
    TriModeMP1_RAID o-- StorageDevice
    TriModeM1_HBA o-- StorageDevice
    SAS_HBA_9500_8e o-- ExternalStorageDevice
    StorageController o-- Firmware
    NetworkInterfaceController o-- PhysicalPort
    NetworkInterfaceController o-- Firmware
    VirtualInterfaceCard o-- PhysicalPort
    VirtualInterfaceCard o-- Firmware
    BaseboardManagementController *-- Firmware
    BaseboardManagementController o-- ServerOperatingSystem
    BaseboardManagementController o-- ManagementSoftware
    ServerOperatingSystem *-- VirtualMachine
    ServerOperatingSystem o-- Firmware
    VirtualMachine o-- ServerOperatingSystem
