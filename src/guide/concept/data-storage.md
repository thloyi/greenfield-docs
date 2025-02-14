---
title: Data Storage
order: 8
---

An object on the Greenfield is stored among multi-SPs like below, for example, 50MB:
<div align=left><img src="../../asset/10-SP-EC.jpg" alt="EC.png" width="700"/></div>

We will introduce some concepts about data storage before describing in detail.

## Segment 
Segment is the basic storage unit of an object. An object payload is composed of one
or many segments in sequence. The segment size is globally configured on the Greenfield
blockchain. The default segment size is 16MB.  For larger objects, the payload data will
be broken into many segments. If the object's size is less than 16MB, it has only one
segment and the segment size is the same as the object's size.

Please note the payload data of an object will be split into the same size segment
but the last segment, which is the actual size. For example, if one object has a size
50MB, only the size of the last segment is 2 MB and the other segments' sizes are all 16MB.

## EC Chunk 
Erasure Code (EC) is introduced to get efficient data redundancy on Greenfield. Segment
is the boundary to perform erasure encoding. Some EC chunks are generated by erasure 
encoding one segment at a time. EC strategy is globally configured on the Greenfield block
chain. The default EC strategy is 4+2, 4 data chunks, and 2 parity chunks for one segment.
The data chunk size is ¼ of the segment. As one typical segment is 16M, one typical data chunk
of EC is 4M.

## Piece
Piece is the basic storage unit for backend storage on Greenfield. Each segment or EC chunk
can be regarded as one data piece. And the key for each piece is generated based on the
policy on the Greenfield chain.

## Pieces' Root Hash
There is one pieces' root hash for the pieces of an object stored in a SP. The pieces' root
hash is part of object meta data on the Greenfield block chain. Each piece's hash is computed
by using hash algorithm on the data piece's content. The pieces' root hash is computed based
on all the pieces' hashes.

## Redundancy Strategy
Redundancy strategy defines how an object payload is stored among SPs, which is globally
configured on the Greenfield blockchain. Below is the current strategy:
* The segment size is 16MB;
* The EC strategy is 4+2;
* All the segment pieces of an object are stored on the Primary SP;
* All the EC chunk pieces of an object are stored on Secondarys SPs, each Secondary SP just
  stores part of them.

## Primary SP
Each bucket on the Greenfield is bound with one SP, which is called primary SP. And the user
needs to select a SP as the primary SP when creating a bucket. For all the objects stored
under the bucket, primary SP will store one complete copy, all segments of the objects’
payload data. And only the primary SP serves users’ read or download requests.

## Secondary SP 
EC chunks of an object payload data are stored on some SPs, which are called secondary SPs.
Each secondary SP stores part of payload data, which is used for better data availability.
The object payload can be recovered from EC chunks.

