P2P wire protocol
=================

This page describe the p2p wire protocol for protocol version 0.3.

Note: Some contents are work in progress and will be implemented before the launch of the mainnet.

Messages
--------

Handshake message
^^^^^^^^^^^^^^^^^

A handshake message is used once at starting handshake. It contains two 4-byte number

~4 : Magic

~8 : Version

Normal message
^^^^^^^^^^^^^^

Header (48bytes) + Payload (variable size)

Message header
^^^^^^^^^^^^^^

~4 : code number of subprotocol . Big endian number

~8 : payload size. Big endian number.

~16 : creation time of this message. unix timestamp with precision of nanosecond . Big endian number

~32 : message id. binary form of uuid.

~48 : original request id. only meaningful if message is response message. binary form of uuid.


Message payload
^^^^^^^^^^^^^^^

Payload is serialized form of protobuf struct. The size and struct type is differ by subprotocol.


List of Subprotocols
--------------------

Code is hexadecimal number.
Refer to `Subprotocols <subprotocols.html>`_ for detailed information of each subprotocol.

+------------------------+------+------------------------------------------------------------------------------------------------------+
|Name                    |Code  |Remark                                                                                                |
+========================+======+======================================================================================================+
|StatusRequest           |  0001|My node status, which includes chainID, address information, last blocks or others. Used in handshake |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|PingRequest             |  0002|Ping including last block hash and number                                                             |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|PingResponse            |  0003|Response ping including last block hash and number                                                    |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|GoAway                  |  0004|Disconnect notice with reason                                                                         |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|AddressesRequest        |  0005|Query request to get list of connected peers                                                          |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|AddressesResponse       |  0006|Response for AddressesRequest.                                                                        |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|GetBlocksRequest        |  0010|                                                                                                      |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|GetBlocksResponse       |  0011|                                                                                                      |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|GetBlockHeadersRequest  |  0012|                                                                                                      |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|GetBlockHeadersResponse |  0013|                                                                                                      |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|NewBlockNotice          |  0016|                                                                                                      |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|GetAncestorRequest      |  0017|                                                                                                      |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|GetAncestorResponse     |  0018|                                                                                                      |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|GetHashesRequest        |  0019|                                                                                                      |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|GetHashesResponse       |  001A|                                                                                                      |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|GetHashByNoRequest      |  001B|                                                                                                      |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|GetHashByNoResponse     |  001C|                                                                                                      |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|GetBlockHeadersResponse |  001D|                                                                                                      |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|GetTXsRequest           |  0020|                                                                                                      |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|GetTxsResponse          |  0021|                                                                                                      |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|NewTxNotice             |  0022|                                                                                                      |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|BlockProducedNotice     |  0030|                                                                                                      |
+------------------------+------+------------------------------------------------------------------------------------------------------+


List of Response Status
-----------------------

Some subprotocols for responsing other message have ResultStatus property.

+------------------------+------+------------------------------------------------------------------------------------------------------+
|Name                    | Code | Remark                                                                                               |
+========================+======+======================================================================================================+
|OK                      |    0 | OK is returned on success.                                                                           |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|CANCELED                |    1 | when operation was canceled                                                                          |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|UNKNOWN                 |    2 |                                                                                                      |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|INVALID_ARGUMENT        |    3 | INVALID_ARGUMENT is missing or wrong value of argument                                               |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|DEADLINE_EXCEEDED       |    4 | timeout                                                                                              |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|NOT_FOUND               |    5 | Resource is not found                                                                                |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|ALREADY_EXISTS          |    6 |                                                                                                      |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|PERMISSION_DENIED       |    7 |                                                                                                      |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|RESOURCE_EXHAUSTED      |    8 |                                                                                                      |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|FAILED_PRECONDITION     |    9 |                                                                                                      |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|ABORTED                 |   10 |                                                                                                      |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|OUT_OF_RANGE            |   11 |                                                                                                      |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|UNIMPLEMENTED           |   12 | indicates operation is not implemented or not supported/enabled in this service.                     |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|INTERNAL                |   13 |                                                                                                      |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|UNAVAILABLE             |   14 | Unavailable indicates the service is currently unavailable.                                          |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|DATA_LOSS               |   15 |                                                                                                      |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|UNAUTHENTICATED         |   16 | indicates the request does not have valid authentication credentials for the operation.              |
+------------------------+------+------------------------------------------------------------------------------------------------------+

Payload of Subprotocols
-----------------------

StatusRequest
^^^^^^^^^^^^^

1. sender: information of sender (address, port, peerID or etc)
2. bestBlockHash: current best block of sender
3. bestHeight: current best block height of sender
4. chainID: ChainID which sender is storing
