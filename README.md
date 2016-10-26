EWP Address Data Types
======================

* [What is the status of this document?][statuses]
* [See the index of all other EWP Specifications][develhub]


Summary
-------

This repository contains XML Schemas used in EWP APIs for street and postal
addressing. It follows the same [versioning rules][compat-rules] as all EWP
APIs do. Other projects are welcome to reuse this schema, along with its
namespace. We are also open to suggestions of extending it.


Address Types
-------------

### Compatibility and conversions

As of 2016, there are no standard addressing schemes. They vary *widely* from
country to country. Even the Universal Postal Union [says so]
[addressing-the-world-paper]. Unfortunately, this means that developers in
each of the partner countries will often have database structures not
strictly compatible with one another, and this in turn MAY mean some
additional work to implement proper conversion, or change your data model.
There's simply no other way.

The format we have [chosen][discussion] allows to specify addresses in two
different ways:

 * **The structural format**. It should allow majority of server developers to
   provide the addresses in a structural form compatible with most current
   European standards (as of 2016), but there is a possibility that it will
   need to be extended in the future.

 * **The simple fallback format**, which leaves a "side door" open for use
   cases we cannot currently predict (i.e. countries which use, or will use,
   different standards). This format follows [best practices][stack-thread]
   used by companies who work with international shipping, so it seems to be a
   reasonable choice for a fallback. (Of the two formats, this one is probably
   *less* likely to become deprecated in the future.)

You may compare both formats by reviewing the attached XSD and the examples
below.


### Street Address vs. Mailing Address

*Street Address* and *Mailing Address* - many of us don't realize that, but for
some recipients there's a big difference between one and another. You can
read on this [here](http://painintheenglish.com/case/3604).

In some EWP APIs we require **server** developers to be aware which type of
address they provide (or we allow them to provide both). **Client** developers
also should have this distinction in mind (for example, should you risk
importing street addresses, if your database expects mailing addresses only?).


The Schema
----------

See attached [`schema.xsd`](schema.xsd) file.


Examples
--------

Two ways of expressing a single address:

```xml
<mailing-address>
    <recipientName>John Doe, Head of IRO Office</recipientName>

    <!-- Option 1: The simple fallback format -->
    <addressLine>Casmir Palace</addressLine>
    <addressLine>Krakowskie Przedmieście 26/28</addressLine>

    <postalCode>00-927</postalCode>
    <locality>Warszawa</locality>
    <country>PL</country>
</mailing-address>

<mailing-address>
    <recipientName>John Doe, Head of IRO Office</recipientName>

    <!-- Option 2: Structural -->
    <buildingNumber>26/28</buildingNumber>
    <buildingName>Casmir Palace</buildingName>
    <streetName>Krakowskie Przedmieście</streetName>

    <postalCode>00-927</postalCode>
    <locality>Warszawa</locality>
    <country>PL</country>
</mailing-address>
```

A valid mailing address, but *not* a valid street address:

```xml
<mailing-address>
    <postOfficeBox>1072</postOfficeBox>
    <postalCode>0316</postalCode>
    <locality>Oslo</locality>
    <country>NO</country>
</mailing-address>
```

A (somewhat) valid street address, but probably *not* a valid mailing address:

```xml
<street-address>
    <addressLine>University of Oslo</addressLine>
    <locality>Oslo</locality>
    <country>NO</country>
</street-address>
```


[develhub]: http://developers.erasmuswithoutpaper.eu/
[statuses]: https://github.com/erasmus-without-paper/ewp-specs-management#statuses
[compat-rules]: https://github.com/erasmus-without-paper/ewp-specs-architecture/#backward-compatibility-rules
[addressing-the-world-paper]: http://www.upu.int/fileadmin/documentsFiles/activities/addressingAssistance/paperAddressingAddressingTheWorldAnAddressForEveryoneEn.pdf
[discussion]: https://github.com/erasmus-without-paper/ewp-specs-architecture/issues/13
[stack-thread]: http://stackoverflow.com/questions/929684/
