[![Build Status](https://travis-ci.com/factsmission/dcat-ap-ch-shacl.svg?branch=master)](https://travis-ci.com/factsmission/dcat-ap-ch-shacl)

# SHACL Shapes for the DCAT Application Profile for Data Portals in Switzerland

This project provides [SHACL](https://www.w3.org/TR/shacl/) to validate metadata against the [eCH-0200 Standard](https://www.ech.ch/vechweb/page?p=dossier&documentNumber=eCH-0200&documentVersion=1.0).

## Status

This project is work in progress. The functionality described in this README might not yet be implemented or not yet work as described.

## Files

 * `ech-0200.shacl.ttl` : This file models the constraints defined in eCH-200 with the SHACL vocabulary
 * `examples/` : This directory contains RDF Turtle files that can be used to test `ech-0200.shacl.ttl`. The conventions is that files ending with `.valid.ttl` will validate and files ending with `.fail.ttl` do not validate.

## Usage

To use the Shapes to validate data you need a SHACL validator such as [TopBraid SHACL API](https://github.com/TopQuadrant/shacl). With this validator you can validate RDF Turtle files as follows:

    shaclvalidate -shapesfile ech-0200.shacl.ttl -datafile data.ttl

For example (from this directory):

    shaclvalidate -shapesfile ech-0200.shacl.ttl -datafile .\examples\minimal.valid.ttl

The example is not strictly minimal as omitting `dcat:themeTaxonomy` would only result in a warning.

## References

This project is similar to and partially based on the [EU DCAT-AP SHACL constraint definitions](https://github.com/SEMICeu/dcat-ap_shacl).

## Note on language

While the eCH-0200 specification is available in German and French the SHACL Shapes are documented in English to better align with other shape files and tools that are likely to be used in combination with it.

## Comments and interpretation of specification

 * The specification mandates the use of schema:url as class. This seems to be a mistake and we assume that it should read schema:URL instead.
 * Inference: the specification is not explicit as to whether and what inference should be allowed. We assume that where vcard:Kind is allowed its subclasses (Individual, Organization, Group, Location) should be allowed to. SHACL only allows specifying ontological statements in the data and not in the Shape-Graph, so currently using a subclass is only accepted if the respective rdfs:subClassOf statement is also present in the data. We could of course explcitly allow some named subclassed in the shape file but this does not seem to be warranted by the spec.
 * The type (`foaf:Document`) does not need to be explicitely specified for a document to validate (the type can be inferred from the rdfs:range of foaf:Document).

 ## Other points to discuss
 * Shouldn't we rather require a Dataset to be named (using standard IRI) rather than requiring a proprietary dct:identifier? Analoguos questions can be asked for `dcat:themeTaxonomy` and `foaf:homepage`.
 * Should xsd:dateTime be supported as well where xsd:date is required?
 * It seems inconsistent to forbid `adms:status` on distributions while generally allowing arbitrary properties.

## License

As prospective part of an [eCH](https://www.ech.ch/) standard, the code and documentations in this repository can be used, distributed and further developed without any restriction by patents or licenses.
