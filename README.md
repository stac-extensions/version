# Versioning Indicators Extension Specification

- **Title:** Versioning Indicators
- **Identifier:** <https://stac-extensions.github.io/version/v1.1.0/schema.json>
- **Field Name Prefix:** -
- **Scope:** Item, Collection
- **Extension [Maturity Classification](https://github.com/radiantearth/stac-spec/tree/master/extensions/README.md#extension-maturity):** Proposal
- **Owner**: @m-mohr
- **History**: [Prior to March 2, 2021](https://github.com/radiantearth/stac-spec/commits/v1.0.0-rc.1/extensions/version)

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
- [ ] Links

| Field Name | Type    | Description |
| ---------- | ------- | ----------- |
| version    | string  | Version of the context this fields is used in (e.g. Asset or Collection). |
| deprecated | boolean | Specifies that the context this field is used in (e.g. Asset or Collection) is deprecated with the potential to be removed. Defaults to `false`. It should be transitioned out of usage as soon as possible and users should refrain from using it in new projects. A link with relation type `latest-version` SHOULD be added to the links and MUST refer to the resource that can be used instead. |

## Relation types

The following types should be used as applicable `rel` types for the
[Link Object](https://github.com/radiantearth/stac-spec/tree/master/item-spec/item-spec.md#link-object)
to reference the latest version, the predecessor version and successor versions.
These are all following [RFC 5829](https://tools.ietf.org/html/rfc5829).

| Type                | Description |
| ------------------- | ----------- |
| latest-version      | This link points to a resource containing the latest (e.g., current) version. |
| predecessor-version | This link points to a resource containing the predecessor version in the version history. |
| successor-version   | This link points to a resource containing the successor version in the version history. |
