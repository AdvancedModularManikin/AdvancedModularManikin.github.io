

## AMM Standard Topic Types
```
module AMM
{
   struct FMA_Location
   {
      long FMAID;
      string name;
   };

   struct UUID
   {
      string id;
   };

   enum AssessmentValue {
      OMISSION_ERROR
      ,COMMISSION_ERROR
      ,EXECUTION_ERROR
      ,SUCCESS
   };
   struct Assessment
   /** QoS:
   *  Reliability: Reliable
   *  Durability: Transient Local
   */
   {
      UUID id;
      UUID event_id;
      AssessmentValue value;
      string comment;
   };

   enum EventAgentType {
      LEARNER
      ,INSTRUCTOR
      ,SCENARIO
      ,PHYSIOLOGY
   };

   struct EventFragment
   /** QoS:
   *  Reliability: Reliable
   *  Durability: Volatile
   */
   {
      UUID id;
      unsigned long long timestamp;
      UUID educational_encounter;
      FMA_Location location;
      EventAgentType agent_type;
      UUID agent_id;
      string type;
      string data;
   };

   struct EventRecord
   /** QoS:
   *  Reliability: Reliable
   *  Durability: Transient Local (In case of module disconnect/reconnect)
   *  Liveliness: Automatic, 1 second lease
   */
   {
      UUID id;
      unsigned long long timestamp;
      UUID educational_encounter;
      FMA_Location location;
      EventAgentType agent_type;
      UUID agent_id;
      string type;
      string data;
   };

   enum FAR_Status {
      REQUESTING
      ,ACCEPTED
      ,REJECTED
   };
   struct FragmentAmendmentRequest
   /** QoS:
   *  Reliability: Reliable
   *  Durability: Volatile
   */
   {
      UUID id;
      UUID fragment_id;
      FAR_Status status;
      // Values that can be amended
      FMA_Location location;
      EventAgentType agent_type;
      UUID agent_id;
   };

   enum LogLevel {
      L_FATAL
      ,L_ERROR
      ,L_WARN
      ,L_INFO
      ,L_DEBUG
      ,L_TRACE
   };
   struct Log
   /** QoS:
   *  Reliability: Reliable
   *  Durability: Transient Local
   */
   {
      unsigned long long timestamp;
      UUID module_id;
      LogLevel level;
      string message;
   };

   struct ModuleConfiguration
   /** QoS:
   *  Reliability: Reliable
   *  Durability: Transient Local
   */
   {
      string name;
      UUID module_id;
      UUID educational_encounter;
      unsigned long long timestamp;
      string capabilities_configuration;
   };

   struct OmittedEvent
   /** QoS:
   *  Reliability: Reliable
   *  Durability: Transient Local (In case of module disconnect/reconnect)
   *  Liveliness: Automatic, 1 second lease
   */
   {
      UUID id;
      unsigned long long timestamp;   // When the omission was detected.
      UUID educational_encounter;
      FMA_Location location;
      EventAgentType agent_type;
      UUID agent_id;
      string type;
      string data;
   };

   struct Semantic_Version {
      unsigned short major;
      unsigned short minor;
      unsigned short patch;
   };
   struct OperationalDescription
   /** QoS:
   *  Reliability: Reliable
   *  Durability: Transient Local
   */
   {
      string name;
      string description;
      string manufacturer;
      string model;
      string serial_number;
      UUID module_id;
      string module_version;
      string configuration_version;
      string AMM_version;
      octet ip_address[4];
      string capabilities_schema; // Defined by CapabilitiesSchema.xsd
   };

   struct PhysiologyModification
   /** QoS:
   *  Reliability: Reliable
   *  Durability: Transient Local
   */
   {
      UUID id;
      UUID event_id;
      string type;
      string data;
   };

   struct PhysiologyValue
   /** QoS:
   *  Reliability: Best Effort
   *  Durability: Transient Local
   *  Ownership: Exclusive
   *  Ownership Strength: Set via Configuration if non-zero
   *  Presentation: Access Scope: Instance, Coherent Access: True, Order Access: False
   *  Liveliness: Automatic, 1 second lease
   */
   {
      UUID educational_encounter;
      long long simulation_time;
      unsigned long long timestamp;
      string name;   // BioGears node path
      string unit;
      double value;
   };

   struct PhysiologyWaveform // Reliable delivery
   /** QoS:
   *  Reliability: Reliable
   *  Durability: Transient Local
   *  Ownership: Exclusive
   *  Ownership Strength: Set via Configuration if non-zero
   *  Liveliness: Automatic, 1 second lease (Lower if feasible, requires testing)
   */
   {
      UUID educational_encounter;
      long long simulation_time;
      unsigned long long timestamp;
      string name;   // BioGears node path
      string unit;
      double value;
   };

   struct RenderModification
   /** QoS:
   *  Reliability: Reliable
   *  Durability: Transient Local
   */
   {
      UUID id;
      UUID event_id;
      string type;
      string data;
   };

   enum ControlType {
      RUN
      ,HALT
      ,RESET
      ,SAVE
   };
   struct SimulationControl
   /** QoS:
   *  Reliability: Reliable
   *  Durability: Transient Local
   *  Liveliness: Automatic, 1 second lease
   */
   {
      unsigned long long timestamp;
      ControlType type;
      UUID educational_encounter;
   };

   enum StatusValue {
      OPERATIONAL
      ,INOPERATIVE
      ,EXIGENT
   };
   struct Status
   /** QoS:
   *  Reliability: Reliable
   *  Durability: Transient Local
   *  Liveliness: Automatic, 1 second lease
   */
   {
      UUID module_id;
      string module_name;
      UUID educational_encounter;
      string capability;
      unsigned long long timestamp;
      StatusValue value;
      string message;
   };
};
```


## Capabilities Schema
```
<?xml version="1.0" encoding="UTF-8"?>
<xs:schema targetNamespace="urn:AdvancedModularManikin"
           xmlns="urn:AdvancedModularManikin"
           xmlns:xs="http://www.w3.org/2001/XMLSchema"
           elementFormDefault="qualified">
  <!-- Capability Glossary Schema -->
  <xs:element name="CapabilityGlossaryDefinition" type="CapabilityGlossaryDefinitionType" />
  <xs:complexType name="CapabilityGlossaryDefinitionType">
    <xs:sequence>
      <xs:element type="CapabilityDefinitionType" name="Capability" minOccurs="1" maxOccurs="unbounded"/>
    </xs:sequence>
  </xs:complexType>
  <xs:complexType name="CapabilityDefinitionType">
    <xs:attributeGroup ref="CapabilityAttributes" />
  </xs:complexType>
  <xs:attributeGroup name="CapabilityAttributes">
    <!-- Add additional attributes as their identified in AMM Capability Types Glossary -->
    <xs:attribute type="xs:string" name="type" use="required"/>
    <xs:attribute type="xs:string" name="location" use="optional"/>
  </xs:attributeGroup>
  <!-- Capabiilities Schema Schema -->
  <xs:element name="CapabilitiesSchema" type="CapabilitiesSchemaType"/>
  <xs:complexType name="CapabilitiesSchemaType">
    <xs:sequence>
      <xs:element type="BaselinePoEPowerType" name="BaselinePoEPower" minOccurs="0"/>
      <xs:element type="CapabilityType" name="Capability" maxOccurs="unbounded"/>
      <xs:element type="ConfigurationType" name="Configuration" minOccurs="0"/>
    </xs:sequence>
  </xs:complexType>
  <xs:complexType name="BaselinePoEPowerType">
    <xs:attribute type="xs:decimal" name="nominal" use="required"/>
    <xs:attribute type="xs:string" name="unit" use="required"/>
  </xs:complexType>
  <xs:complexType name="CapabilityType">
    <xs:sequence>
      <xs:element type="TopicsType" name="Subscriptions"/>
      <xs:element type="TopicsType" name="Publications"/>
      <xs:element type="AssessmentsType" name="Assessments"/>
      <xs:element type="ResourcesType" name="Resources" />
    </xs:sequence>
    <xs:attributeGroup ref="CapabilityAttributes"/>
    <xs:anyAttribute/>
  </xs:complexType>
  <xs:complexType name="TopicsType">
    <xs:sequence>
      <!-- This list should match the Topics defined in AMM.idl -->
      <xs:element type="AssessmentType" name="Assessment" maxOccurs="unbounded" minOccurs="0"/>
      <xs:element type="EventFragmentType" name="EventFragment" maxOccurs="unbounded" minOccurs="0"/>
      <xs:element type="EventRecordType" name="EventRecord" maxOccurs="unbounded" minOccurs="0"/>
      <xs:element type="FragmentAmendmentRequestType" name="FragmentAmendmentRequest" maxOccurs="unbounded"
                  minOccurs="0"/>
      <xs:element type="LogType" name="Log" maxOccurs="unbounded" minOccurs="0"/>
      <xs:element type="ModuleConfigurationType" name="ModuleConfiguration" maxOccurs="unbounded" minOccurs="0"/>
      <xs:element type="OmittedEventType" name="OmittedEvent" maxOccurs="unbounded" minOccurs="0"/>
      <xs:element type="OperationalDescriptionType" name="OperationalDescription" maxOccurs="unbounded" minOccurs="0"/>
      <xs:element type="PhysiologyModificationType" name="PhysiologyModification" maxOccurs="unbounded" minOccurs="0"/>
      <xs:element type="PhysiologyValueType" name="PhysiologyValue" maxOccurs="unbounded" minOccurs="0"/>
      <xs:element type="PhysiologyWaveformType" name="PhysiologyWaveform" maxOccurs="unbounded" minOccurs="0"/>
      <xs:element type="RenderModificationType" name="RenderModification" maxOccurs="unbounded" minOccurs="0"/>
      <xs:element type="SimulationControlType" name="SimulationControl" maxOccurs="unbounded" minOccurs="0"/>
      <xs:element type="StatusType" name="Status" maxOccurs="unbounded" minOccurs="0"/>
      <xs:any namespace="##other" processContents="lax" maxOccurs="unbounded" minOccurs="0"/>
    </xs:sequence>
    <xs:anyAttribute/>
  </xs:complexType>
  <xs:complexType name="AssessmentsType">
    <xs:sequence>
      <xs:element type="EventRecordType" name="EventRecord" maxOccurs="unbounded" minOccurs="0"/>
    </xs:sequence>
    <xs:anyAttribute/>
  </xs:complexType>
  <xs:complexType name="ResourcesType" mixed="true">
    <xs:sequence>
      <xs:element type="RequirementType" name="Requirement" maxOccurs="unbounded" minOccurs="0"/>
      <xs:element type="SupplyType" name="Supply" maxOccurs="unbounded" minOccurs="0"/>
    </xs:sequence>
    <xs:anyAttribute/>
  </xs:complexType>
  <xs:complexType name="AssessmentType">
    <xs:anyAttribute/>
  </xs:complexType>
  <xs:complexType name="EventFragmentType">
    <xs:attributeGroup ref="EventAttributes"/>
    <xs:anyAttribute/>
  </xs:complexType>
  <xs:complexType name="EventRecordType">
    <xs:attributeGroup ref="EventAttributes"/>
    <xs:anyAttribute/>
  </xs:complexType>
  <xs:complexType name="FragmentAmendmentRequestType">
    <xs:attributeGroup ref="EventAttributes"/>
    <xs:anyAttribute/>
  </xs:complexType>
  <xs:complexType name="LogType">
    <xs:attribute type="LogLevel" name="level" use="optional"/>
    <xs:anyAttribute/>
  </xs:complexType>
  <xs:complexType name="ModuleConfigurationType">
    <xs:anyAttribute/>
  </xs:complexType>
  <xs:complexType name="OmittedEventType">
    <xs:attributeGroup ref="EventAttributes"/>
  </xs:complexType>
  <xs:complexType name="OperationalDescriptionType">
    <xs:anyAttribute/>
  </xs:complexType>
  <xs:complexType name="PhysiologyModificationType">
    <xs:attributeGroup ref="PhysiologyModificationsAttributes"/>
    <xs:anyAttribute/>
  </xs:complexType>
  <xs:complexType name="PhysiologyValueType">
    <xs:attributeGroup ref="PhysiologyDataAttributes"/>
    <xs:anyAttribute/>
  </xs:complexType>
  <xs:complexType name="PhysiologyWaveformType">
    <xs:attributeGroup ref="PhysiologyDataAttributes"/>
    <xs:anyAttribute/>
  </xs:complexType>
  <xs:complexType name="RenderModificationType">
    <xs:attributeGroup ref="RenderModificationAttributes"/>
    <xs:anyAttribute/>
  </xs:complexType>
  <xs:complexType name="SimulationControlType">
    <xs:attribute type="ControlType" name="type" use="optional"/>
    <xs:anyAttribute/>
  </xs:complexType>
  <xs:complexType name="StatusType">
    <xs:attribute type="StatusValue" name="value" use="optional"/>
    <!-- TODO: Change from generic string to enum generated from Capability Types Glossary -->
    <xs:attribute type="xs:string" name="capability" use="required"/>
    <xs:anyAttribute/>
  </xs:complexType>
  <xs:complexType name="RequirementType">
    <xs:attribute type="ResourceType" name="type" use="required"/>
    <xs:attribute type="xs:string" name="peak" use="optional"/>
    <xs:attribute type="xs:string" name="nominal" use="required"/>
    <xs:attribute type="xs:string" name="unit" use="required"/>
    <xs:anyAttribute/>
  </xs:complexType>
  <xs:complexType name="SupplyType">
    <xs:attribute type="ResourceType" name="type" use="required"/>
    <xs:attribute type="xs:string" name="capacity" use="required"/>
    <xs:attribute type="xs:string" name="unit" use="required"/>
  </xs:complexType>
  <xs:attributeGroup name="EventAttributes">
    <!-- 'match' attribute defines whether the module requires a matching publisher/subscriber during operation -->
    <xs:attribute type="MatchType" name="match" default="optional"/>
    <!-- TODO: Change from generic string to enum generated from Event Record Types Glossary -->
    <xs:attribute type="xs:string" name="type" use="required"/>
  </xs:attributeGroup>
  <xs:attributeGroup name="PhysiologyDataAttributes">
    <!-- 'match' attribute defines whether the module requires a matching publisher/subscriber during operation -->
    <xs:attribute type="MatchType" name="match" default="optional"/>
    <!-- TODO: Change from generic string to enum generated from CDM data -->
    <xs:attribute type="xs:string" name="name" use="required"/>
  </xs:attributeGroup>
  <xs:attributeGroup name="PhysiologyModificationsAttributes">
    <!-- 'match' attribute defines whether the module requires a matching publisher/subscriber during operation -->
    <xs:attribute type="MatchType" name="match" default="optional"/>
    <!-- TODO: Change from generic string to enum generated from CDM data -->
    <xs:attribute type="xs:string" name="type" use="required"/>
  </xs:attributeGroup>
  <xs:attributeGroup name="RenderModificationAttributes">
    <!-- 'match' attribute defines whether the module requires a matching publisher/subscriber during operation -->
    <xs:attribute type="MatchType" name="match" default="optional"/>
    <!-- TODO: Change from generic string to enum generated from Render Modification Types Glossary -->
    <xs:attribute type="xs:string" name="type" use="required"/>
  </xs:attributeGroup>
  <xs:simpleType name="MatchType">
    <xs:restriction base="xs:string">
      <xs:enumeration value="required"/>
      <xs:enumeration value="optional"/>
    </xs:restriction>
  </xs:simpleType>
  <xs:simpleType name="LogLevel">
    <xs:restriction base="xs:string">
      <xs:enumeration value="FATAL"/>
      <xs:enumeration value="ERROR"/>
      <xs:enumeration value="WARN"/>
      <xs:enumeration value="INFO"/>
      <xs:enumeration value="DEBUG"/>
      <xs:enumeration value="TRACE"/>
    </xs:restriction>
  </xs:simpleType>
  <xs:simpleType name="ControlType">
    <xs:restriction base="xs:string">
      <xs:enumeration value="RUN"/>
      <xs:enumeration value="HALT"/>
      <xs:enumeration value="RESET"/>
      <xs:enumeration value="SAVE"/>
    </xs:restriction>
  </xs:simpleType>
  <xs:simpleType name="StatusValue">
    <xs:restriction base="xs:string">
      <xs:enumeration value="OPERATIONAL"/>
      <xs:enumeration value="INOPERATIVE"/>
      <xs:enumeration value="EXIGENT"/>
    </xs:restriction>
  </xs:simpleType>
  <xs:simpleType name="ResourceType">
    <xs:restriction base="xs:string">
      <xs:enumeration value="Power"/>
      <xs:enumeration value="Blood Simulant"/>
      <xs:enumeration value="Clear Liquid"/>
      <xs:enumeration value="Compressed Air"/>
    </xs:restriction>
  </xs:simpleType>
  <xs:complexType name="ConfigurationType">
    <xs:sequence>
      <xs:element type="CapabilityConfigurationType" name="Capability" maxOccurs="unbounded" minOccurs="0"/>
    </xs:sequence>
  </xs:complexType>
  <xs:complexType name="CapabilityConfigurationType">
    <xs:choice>
      <xs:element name="hex" type="xs:hexBinary"/>
      <xs:element name="base64" type="xs:base64Binary"/>
      <!-- If configuration isn't binary, modules should create an XML Schema of the configuration options in order to
        facilitate future tooling, when feasible. -->
      <xs:any namespace="##other" processContents="lax"/>
    </xs:choice>
    <xs:attributeGroup ref="CapabilityAttributes"/>
    <xs:anyAttribute/>
  </xs:complexType>
</xs:schema>
```
