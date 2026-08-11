# Versioning Indicators Extension Specification

- **Title:** Versioning Indicators
- **Identifier:** <https://stac-extensions.github.io/version/v1.2.0/schema.json>
- **Field Name Prefix:** -
- **Scope:** Item, Collection
- **Extension [Maturity Classification](https://github.com/radiantearth/stac-spec/tree/master/extensions/README.md#extension-maturity):** Candidate
- **Owner**: @m-mohr

This document explains the Versioning Indicators Extension to the
[SpatioTemporal Asset Catalog](https://github.com/radiantearth/stac-spec) (STAC) specification.

This extension allows to version STAC Collections and STAC Items. Therefore, it also allows to deprecate legacy versions.
Only fields and possible link relation types are defined in this extension,
but it does NOT suggest any versioning best practices to structure static or dynamic catalogs.
Instead check the [Versioning Best Practices for Catalogs](https://github.com/radiantearth/stac-spec/tree/master/best-practices.md#versioning-for-catalogs).

- Examples:
  - [Item example](examples/item.json): Shows the basic usage of the extension in a STAC Item
  - [Collection example](examples/collection.json): Shows the basic usage of the extension in a STAC Collection
- [JSON Schema](json-schema/schema.json)
- [Changelog](./CHANGELOG.md)

## Fields

The fields in the table below can be used in these parts of STAC documents:
- [x] Catalogs
- [x] Collections
- [x] Item Properties (incl. Summaries in Collections)
- [x] Assets (for both Collections and Items, incl. Item Asset Definitions in Collections)
- [x] Links

| Field Name   | Type    | Description |
| ------------ | ------- | ----------- |
| version      | string  | Version of the context this fields is used in (e.g. Asset or Collection). |
| deprecated   | boolean | Specifies that the context this field is used in (e.g. Asset or Collection) is deprecated with the potential to be removed. Defaults to `false`. It should be transitioned out of usage as soon as possible and users should refrain from using it in new projects. A link with relation type `latest-version` SHOULD be added to the links and MUST refer to the resource that can be used instead. |
| experimental | boolean | Specifies that the context this field is used in (e.g. Asset or Collection) is experimental with the potential to break or be unstable. Defaults to `false`. |

These fields have different meaning depending on where they are used. When used as an Item properties or top-level Collection field, they refer to
the version or deprecation of all data referenced in the Item or Collection, which may include the metadata itself.
When used in an Asset Object, they refer to the version or deprecation
of the particular data asset linked to in the Asset Object.

## Relation types

The following types should be used as applicable `rel` types for the
[Link Object](https://github.com/radiantearth/stac-spec/tree/master/item-spec/item-spec.md#link-object)
to describe the relationships between the versions of a resource.
The relation types `latest-version`, `predecessor-version`, `successor-version`, and `version-history`
follow [RFC 5829](https://tools.ietf.org/html/rfc5829).
The relation types `has-version` and `is-version-of` are inspired by DataCite,
see the [mapping to DataCite](#mapping-to-datacite) below.

| Type                | Description |
| ------------------- | ----------- |
| latest-version      | This link points to a STAC resource containing the latest (e.g., current) version. A maximum of one link can use this relation type per STAC resource. |
| predecessor-version | One or multiple links can point to STAC resources that are predecessor versions in the version history. |
| successor-version   | One or multiple links can point to STAC resources that are successor versions in the version history. |
| version-history     | This link points to a version history or changelog. This can be for example a Markdown file with the corresponding media type, or a STAC Catalog or Collection. |
| has-version         | One or multiple links can point to resources that are versions of this resource. Typically used on an overarching entity (e.g., a version-independent resource) that groups versioned resources. |
| is-version-of       | This link points to the overarching entity (e.g., a version-independent resource) of which this resource is a version. This is the reverse of `has-version`. |

Note that the `canonical` relation type as described in the
[STAC best practices](https://github.com/radiantearth/stac-spec/blob/master/best-practices.md#using-relation-types)
is not a versioning relation type.
It points from a copy to the primary location of the *same* version of a resource,
whereas `is-version-of` points to the version-independent (overarching) entity.

### Mapping to DataCite

The following table maps the relation types of this extension to the corresponding DataCite
[relationType](https://datacite-metadata-schema.readthedocs.io/en/latest/appendices/appendix-1/relationType/) values.
The direction of the relations is the same:
the DataCite relationType applies to the resource that contains the link,
and the link target corresponds to the related resource in DataCite.

| STAC relation type  | DataCite relationType |
| ------------------- | --------------------- |
| latest-version      | n/a                   |
| predecessor-version | `IsNewVersionOf`      |
| successor-version   | `IsPreviousVersionOf` |
| version-history     | n/a                   |
| has-version         | `HasVersion`          |
| is-version-of       | `IsVersionOf`         |

## Contributing

All contributions are subject to the
[STAC Specification Code of Conduct](https://github.com/radiantearth/stac-spec/blob/master/CODE_OF_CONDUCT.md).
For contributions, please follow the
[STAC specification contributing guide](https://github.com/radiantearth/stac-spec/blob/master/CONTRIBUTING.md) Instructions
for running tests are copied here for convenience.

### Running tests

The same checks that run as checks on PR's are part of the repository and can be run locally to verify that changes are valid. 
To run tests locally, you'll need `npm`, which is a standard part of any [node.js installation](https://nodejs.org/en/download/).

First you'll need to install everything with npm once. Just navigate to the root of this repository and on 
your command line run:
```bash
npm install
```

Then to check markdown formatting and test the examples against the JSON schema, you can run:
```bash
npm test
```

This will spit out the same texts that you see online, and you can then go and fix your markdown or examples.

If the tests reveal formatting problems with the examples, you can fix them with:
```bash
npm run format-examples
```
