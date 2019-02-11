# Summary

As described elsewhere Jet messages are encoded as JSON documents.
These documents need to be passed from sender to receiver using the underlying transport layer.

Jet by itself is independent from the transport layer.
Nevertheless, on stream-oriented connections, there is the necessity to separate an incoming stream of characters
into the distinct JSON documents it is composed from.
Some transport layers are able to transfer the document framing inherently, while others are not.

# Document Framing Implementation

In case the transport layer is solely stream oriented a document framing is introduced at the "transport layer binding" level.

This is always done according to the following definition:

## length-prefixed

The sender prefixes each JSON document (aka message) with the length of the message.

* The length is allway encoded in four octets (aka bytes)
* The four octets encode an unsigned integer, in binary encoding
* The order of the four octets is network-byte-order
* The number encoded is the length of the JSON document, not including the four length octets.

Remarks:
* The length might include whitespace in the JSON document, including heading (before the first '''{''') and trailing (following after the last '''}'''

# Transport Layers and Jet Message Framing

| Transport | Document Framing     |
| --------- | -------------------- |
| TCP       | length-prefixed      |
| WebSocket | no framing, since the WebSocket-protocol is document-oriented |
| RS-232    | length-prefixed |

