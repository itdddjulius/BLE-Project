# The Client’s Specification

<img align="left" src="res/logo/eddystone_logo.png" hspace="15" style="float: left">Here is the client’s protocol specification that defines a Bluetooth low energy (BLE) message format for proximity beacon messages. It describes several different frame types that may be used individually or in combinations to create beacons that can be used for a variety of applications.

## Design Goals

The design for the client needs to be driven by several key goals:

- Works well with Android and iOS Bluetooth developer APIs
- Straightforward implementation on a wide range of existing BLE devices
- Flexible architecture permitting development of new frame types
- Fully compliant with the Bluetooth Core Specification

## Protocol Specification

The common frame PDU types and the individual service data byte layouts for
the -UID, -URL and -TLM frame formats are documented as follows:

Design Goals

The design for the client needs to be driven by several key goals:

Works well with Android and iOS Bluetooth developer APIs
Straightforward implementation on a wide range of existing BLE devices
Flexible architecture permitting development of new frame types
Fully compliant with the Bluetooth Core Specification
Common Elements

Every frame type must contain the following PDU data types:

The Complete List of 16-bit Service UUIDs as defined in The Bluetooth Core Specification Supplement (CSS) v5, Part A, § 1.1. The Complete List of 16-bit Service UUIDs must contain the Service UUID of 0xFEAA. This is included to allow background scanning on iOS devices.
The Service Data data type, Ibid., § 1.11. The Service Data - 16 bit UUID data type must be the Service UUID of 0xFEAA.
Frame Type

High-Order 4 bits

Byte Value

UID

0000
0x00
URL
0001
0x10
TLM
0010
0x20
RESERVED
0011
0x30
RESERVED
0100
0x40
 

The specific type of frame is encoded in the high-order four bits of the first octet in the Service Data associated with the Service UUID. Permissible values are:

The four low-order bits are reserved for future use and must be 0000.
Note: although the core Bluetooth data type are defined in the standard as little-endian, multi-value bytes defined in the Service Data are all big-endian.

An example frame type may look like:

Byte offset

Value

Description

Data Type

0

0x02
Length
Flags. CSS v5, Part A, § 1.3
1
0x01
Flags data type value
2
0x06
Flags data
3
0x03
Length
Complete list of 16-bit Service UUIDs. Ibid. § 1.1
4
0x03
Complete list of 16-bit Service UUIDs data type value
5
0xAA
16-bit UUID
6
0xFE
...
7
0x??
Length
Service Data. Ibid. § 1.11
8
0x16
Service Data data type value
9
0xAA
16-bit UUID
10
0xFE
...
With subsequent bytes comprising the frame-specific Service Data.

The individual frame types are listed below.

UID

The UID frame broadcasts an opaque, unique 16-byte Beacon ID composed of a 10-byte namespace and a 6-byte instance. The Beacon ID may be useful in mapping a device to a record in external storage. The namespace portion of the ID may be used to group a particular set of beacons, while the instance ID identifies individual devices in the group. The division of the ID into namespace and instance components may also be used to optimize BLE scanning strategies, e.g. by filtering only on the namespace.

URL

The URL frame broadcasts a URL using a compressed encoding format in order to fit more within the limited advertisement packet.

Once decoded, the URL can be used by any client with access to the internet. For example, if an URL beacon were to broadcast the URL https://goo.gl/Aq18zF, then any client that received this packet could choose to visit that url.
The URL frame forms the backbone of the Physical Web, an effort to enable frictionless discovery of web content relating to one’s surroundings.

URL incorporates all of the learnings from the UriBeacon format from which it evolved.

TLM

The TLM frame broadcasts telemetry information about the beacon itself such as battery voltage, device temperature, and counts of broadcast packets.

 
Well I know this sounds quite complex, however I have recently worked on a project for a client performing BLE Payment Gateways (Visa & MasterCard version of iPay), with much similar requirements, so the coding and technology I know.


## Tools and Code Samples

We have a variety of tools and code samples to assist developers and implementors in working with devices.

- An Android app that performs non-exhaustive basic validation of all the frame types is available at [tools/eddystone-validator](tools/-validator).

- An Android app that can broadcast configurable -UID frames is available at [eddystone-uid/tools/txeddystone-uid](-uid/tools/txeddystone-uid).

- A validation tool for the dedicated Eddystone-URL configuration service is at
[eddystone-url/tools/eddystone-url-config-validator](eddystone-url/tools/eddystone-url-config-validator).
