<!-- SPDX-License-Identifier: CC-BY-4.0 -->

# Provider Extension Block Examples

This page is non-normative. The normative extension rules are in [../SPEC.md](../SPEC.md).

OAA extension blocks let providers and applications preserve metadata that is not defined as portable OAA base data.

For normative extension-container and extension-block behavior, see [../SPEC.md](../SPEC.md). In general, extension blocks use reverse-DNS-style names controlled by the writer or provider, and readers that do not understand an extension block can still locate archive files through base OAA fields.

All identifiers, URLs, titles, names, prices, and dates below are illustrative. They are not real collector, account, artwork, transaction, or source-image data.

The examples in this file are recommended shapes for currently observed provider data. They are not official provider addenda unless the relevant provider adopts them.

The JSON snippets below are partial manifest fragments. They show the surrounding OAA field that contains the extension block, not complete manifests. Optional OAA base fields are omitted unless they explain an extension mapping, conflict, or placement rule. Provider and application extension blocks are intentionally more detailed so implementers can see the kinds of fields each provider or application may preserve.

OAA is intended to be an open interchange format so original art collections can move between, and be used with, multiple providers and applications. To preserve that portability, writers should use base OAA fields wherever possible. If useful shared metadata is missing from the base specification and is not truly provider-specific, providers and implementers are encouraged to use the [proposal process](../proposals/) to suggest a portable OAA field. Extension blocks exist so providers do not need to wait for proposal acceptance, and so genuinely provider-specific data, such as CAF Illustration Exchange fields, can still be preserved. The bottom line: prefer base OAA values whenever possible; use extension fields for provider-specific data, application-specific bookkeeping, and provisional experiments.

When creating an OAA archive from an application database, a writer should use this placement rule:

- Portable OAA metadata goes in base OAA fields.
- Stable provider object identifiers and canonical object URLs go in `external_links`.
- Provider-specific controlled IDs, source-image details, and provider-backed metadata fields not yet represented by base OAA fields go in provider extension blocks.
- Application implementation metadata goes in an application-owned extension block, such as `app.oa-curator`.
- Extension blocks should not echo values already carried by OAA base fields.
- Absolute local filesystem paths, such as source folders, linked-file paths, current file paths, and derivative output paths, must not be emitted into OAA manifests.

Recommended provider keys:

| Provider or application | Extension key |
| --- | --- |
| ComicArtFans | `com.comicartfans` |
| SNIKT.com | `com.snikt` |
| Raremarq | `com.raremarq` |
| OA Curator | `app.oa-curator` |

## ComicArtFans

ComicArtFans data maps to OAA through collection, gallery, artwork, file, public metadata, and private metadata fragments. CAF gallery rooms are identified with `GSub`; CAF piece identity belongs in artwork-level or file-level `external_links`.

### Collection, Gallery, And Artwork Identity

When the OAA base external link fields satisfy the provider need, use those fields directly. Do not add a CAF extension block only to repeat `external_links[].id`.

Collection-level CAF IDs map to collection-level `external_links`.

```json
{
  "id": "collection-example",
  "name": "Example Collection",
  "external_links": [
    {
      "provider": "com.comicartfans",
      "id": "example-gcat",
      "url": "https://example.com/comicartfans/gallery/example-gcat"
    }
  ]
}
```

CAF gallery rooms map to gallery-level `external_links`.

```json
{
  "id": "gallery-example",
  "name": "Example Gallery",
  "external_links": [
    {
      "provider": "com.comicartfans",
      "id": "example-gsub",
      "url": "https://example.com/comicartfans/gallery-room/example-gsub"
    }
  ]
}
```

CAF piece IDs map to artwork-level `external_links`.

```json
{
  "id": "artwork-example-001",
  "title": "Example Artwork Title",
  "external_links": [
    {
      "provider": "com.comicartfans",
      "id": "example-caf-piece",
      "url": "https://example.com/comicartfans/piece/example-caf-piece"
    }
  ]
}
```

### Public Metadata And Artist Roles

Portable public fields should use base OAA fields. CAF controlled IDs remain in `public_metadata.extensions.com.comicartfans`; CAF fields that only repeat base title, description, artist, role, or visibility values should not be stored as duplicate extension data. CAF readers should translate base OAA artist roles, such as `Penciller` and `Inker`, to internal CAF artist job IDs as needed.

```json
{
  "public_metadata": {
    "publication_status": "published_art",
    "artist_credits": [
      {
        "display_name": "Example Artist",
        "first_name": "Example",
        "last_name": "Artist",
        "role": "Penciller"
      },
      {
        "display_name": "Example Inker",
        "first_name": "Example",
        "last_name": "Inker",
        "role": "Inker"
      }
    ],
    "extensions": {
      "com.comicartfans": {
        "media_type_id": "7",
        "art_type_id": "1",
        "publication_status_id": "1",
        "illustration_exchange": false,
        "ix_for_sale": false
      }
    }
  }
}
```

CAF `publication_status_id` values can represent CAF-specific publication states. The portable OAA base values are controlled as `published_art` and `unpublished_art`; CAF-specific states such as `CAF Member Art` should remain in `public_metadata.extensions.com.comicartfans.publication_status`. When the CAF extension block uses `publication_status`, omit base `public_metadata.publication_status`.

```json
{
  "public_metadata": {
    "extensions": {
      "com.comicartfans": {
        "publication_status_id": "3",
        "publication_status": "CAF Member Art"
      }
    }
  }
}
```

### Private Metadata

Private collection-management metadata should use base OAA private fields. CAF may store provider-side fields for these values, but writers should not copy the same values into a CAF extension block only to mirror the base OAA values.

```json
{
  "private_metadata": {
    "purchase_price": "100.00 USD",
    "estimated_value": "150.00 USD",
    "purchase_date": "2026-01-01",
    "provenance": "Example provenance note.",
    "personal_notes": "Example private collector note."
  }
}
```

For normalized OAA metadata, keep private collection-management values in base OAA fields.

### File Source Data And Format Tiers

CAF file source URLs can be represented as file-level `external_links` entries. CAF-specific file source details can be preserved in the external link's `extensions.com.comicartfans` block.

As defined in [../SPEC.md](../SPEC.md), extension blocks do not override `external_links[].url`. If CAF source data needs to preserve additional source URLs, use distinct secondary fields such as `source_image_url`, `thumbnail_url`, or `master_image_url`.

```json
{
  "files": [
    {
      "id": "file-1",
      "relative_path": "example-artwork.png",
      "file_kind": "raw",
      "external_links": [
        {
          "provider": "com.comicartfans",
          "id": "example-caf-piece:primary",
          "url": "https://example.com/comicartfans/images/example-caf-piece-primary.png",
          "extensions": {
            "com.comicartfans": {
              "thumbnail_url": "https://example.com/comicartfans/images/example-caf-piece-thumb.png",
              "image_index": 1
            }
          }
        }
      ],
      "extensions": {
        "com.comicartfans": {
          "format_tier": "premium"
        }
      }
    }
  ]
}
```

CAF derivative assets use ordinary OAA file entries with `file_kind: "derivative"`. CAF-specific export tier data belongs in `extensions.com.comicartfans`.

```json
{
  "files": [
    {
      "id": "file-2",
      "relative_path": "example-artwork-caf-basic.png",
      "file_kind": "derivative",
      "extensions": {
        "com.comicartfans": {
          "format_tier": "basic"
        }
      }
    }
  ]
}
```

Common CAF `format_tier` values include `basic` and `premium`. These are CAF compatibility values, not portable OAA `image_role` values.

## SNIKT.com

SNIKT.com currently places all pieces in one user gallery, which has no observable name or index and effectively shadows the collection. SNIKT.com public and private metadata fields belong in `public_metadata.extensions.com.snikt` or `private_metadata.extensions.com.snikt` when base OAA fields do not already represent them. SNIKT may use the same fields in its own interfaces, but the fields represent SNIKT-backed collection metadata rather than UI instructions.

### Collection, Gallery, Artwork, And File Identity

Collection-level SNIKT user identity maps to collection-level `external_links`. Do not add a SNIKT extension block only to repeat `external_links[].id`.

```json
{
  "id": "collection-example",
  "name": "Example Collection",
  "external_links": [
    {
      "provider": "com.snikt",
      "id": "example-snikt-user",
      "url": "https://example.com/snikt/user/example-snikt-user"
    }
  ]
}
```

SNIKT.com does not expose a distinct gallery name or gallery index. If a writer includes a gallery-level SNIKT link, it should only point to the same observable user gallery and should not invent SNIKT-specific gallery metadata.

```json
{
  "id": "gallery-example",
  "name": "Example Gallery",
  "external_links": [
    {
      "provider": "com.snikt",
      "id": "example-snikt-user",
      "url": "https://example.com/snikt/user/example-snikt-user"
    }
  ]
}
```

SNIKT artwork identity maps to artwork-level `external_links`.

```json
{
  "id": "artwork-example-001",
  "title": "Example Artwork Title",
  "external_links": [
    {
      "provider": "com.snikt",
      "id": "example-snikt-image",
      "url": "https://example.com/snikt/image/example-snikt-image",
      "extensions": {
        "com.snikt": {
          "document_id": "example-snikt-document"
        }
      }
    }
  ]
}
```

SNIKT source image metadata can also be preserved on file-level external links.

```json
{
  "files": [
    {
      "id": "file-1",
      "relative_path": "example-artwork.png",
      "file_kind": "raw",
      "external_links": [
        {
          "provider": "com.snikt",
          "id": "example-snikt-image:primary",
          "url": "https://example.com/snikt/image/example-snikt-image",
          "extensions": {
            "com.snikt": {
              "image_index": 1
            }
          }
        }
      ]
    }
  ]
}
```

### Public And Private Metadata

Writers can emit tracked SNIKT metadata in normalized snake-case form. Values already carried by base OAA title, description, or artist-credit fields should not be stored again in the SNIKT extension block. SNIKT readers should translate base OAA artist roles, such as `Penciller` and `Inker`, to internal SNIKT role state as needed.

SNIKT exposes different metadata fields depending on the artwork type. Writers should omit optional SNIKT fields that are unknown, unsupported for the artwork type, or already represented by OAA base fields. The examples below show recommended populated metadata shapes for each currently observed SNIKT art type.

Use base `public_metadata.for_sale_status` for portable availability text. SNIKT-specific price and offer fields may remain in the SNIKT extension block.

Cover example:

```json
{
  "public_metadata": {
    "artist_credits": [
      {
        "display_name": "Example Artist",
        "first_name": "Example",
        "last_name": "Artist",
        "role": "Penciller"
      },
      {
        "display_name": "Example Inker",
        "first_name": "Example",
        "last_name": "Inker",
        "role": "Inker"
      }
    ],
    "extensions": {
      "com.snikt": {
        "metadata": {
          "art_type": "Cover",
          "comic_publisher": "Example Publisher",
          "series_title": "Example Cover Series",
          "issue_number": "5",
          "year": "1940",
          "other": "Published cover example.",
          "tags": "cover, example",
          "is_nsfw": false,
          "price": "2500.00 USD",
          "is_open_to_offers": true
        }
      }
    }
  }
}
```

Interior example:

```json
{
  "public_metadata": {
    "extensions": {
      "com.snikt": {
        "metadata": {
          "art_type": "Interior",
          "comic_publisher": "Example Publisher",
          "series_title": "Example Interior Series",
          "issue_number": "1",
          "series_page_number": "23",
          "year": "2025",
          "other": "Story page with a full-page action sequence.",
          "tags": "interior-page, example",
          "is_nsfw": false,
          "is_open_to_offers": false
        }
      }
    }
  }
}
```

Commission example:

```json
{
  "public_metadata": {
    "extensions": {
      "com.snikt": {
        "metadata": {
          "art_type": "Commission",
          "character": "Example Commission Character",
          "year": "2026",
          "other": "Private collector commission.",
          "tags": "commission, example",
          "is_nsfw": false,
          "is_open_to_offers": false
        }
      }
    }
  }
}
```

Trading Card Art example:

```json
{
  "public_metadata": {
    "extensions": {
      "com.snikt": {
        "metadata": {
          "art_type": "Trading Card Art",
          "series_title": "Example Trading Card Series",
          "character": "Example Trading Card Character",
          "year": "1994",
          "other": "Published trading card painting.",
          "tags": "trading-card, example",
          "is_nsfw": false,
          "is_open_to_offers": true
        }
      }
    }
  }
}
```

Animation Cel example:

```json
{
  "public_metadata": {
    "extensions": {
      "com.snikt": {
        "metadata": {
          "art_type": "Animation Cel",
          "subcategory": "Production Cel",
          "series_title": "Example Animated Series",
          "episode_number": "12",
          "episode_title": "The Lost Sketch",
          "animation_studio": "Example Animation Studio",
          "year": "1998",
          "character": "Example Hero",
          "tags": "animation-cel, production-art",
          "is_nsfw": false,
          "is_open_to_offers": false
        }
      }
    }
  }
}
```

Comic Strip example:

```json
{
  "public_metadata": {
    "extensions": {
      "com.snikt": {
        "metadata": {
          "art_type": "Comic Strip",
          "series_title": "Example Daily Strip",
          "strip_title": "Sunday Morning",
          "published_date": "1956-04-15",
          "is_sunday_strip": true,
          "year": "1956",
          "other": "Sunday strip tier with hand-lettered title.",
          "tags": "comic-strip, sunday",
          "is_nsfw": false,
          "is_open_to_offers": false
        }
      }
    }
  }
}
```

Other Illustration example:

```json
{
  "public_metadata": {
    "extensions": {
      "com.snikt": {
        "metadata": {
          "art_type": "Other Illustration",
          "character": "Original Fantasy Character",
          "year": "2026",
          "other": "Standalone convention illustration.",
          "tags": "illustration, fantasy",
          "is_nsfw": true,
          "is_open_to_offers": false
        }
      }
    }
  }
}
```

SNIKT estimated value is not provider-specific. Writers should put it in base `private_metadata.estimated_value` and should not duplicate it in `private_metadata.extensions.com.snikt`.

## Raremarq

Raremarq data maps to OAA through user, gallery, piece, public metadata, source-image, listing, commerce, certification, and draft-image fragments. These fields represent data Raremarq tracks, regardless of how Raremarq receives or uses it; writers should emit only fields they know or fields they can safely leave blank.

### Collection, Gallery, Artwork, And File Identity

Collection-level Raremarq user identity maps to collection-level `external_links`. Do not add a Raremarq extension block only to repeat `external_links[].id`.

```json
{
  "id": "collection-example",
  "name": "Example Collection",
  "external_links": [
    {
      "provider": "com.raremarq",
      "id": "example-raremarq-user",
      "url": "https://example.com/raremarq/u/example-raremarq-user"
    }
  ]
}
```

Raremarq gallery identity maps to gallery-level `external_links`.

```json
{
  "id": "gallery-example",
  "name": "Example Gallery",
  "external_links": [
    {
      "provider": "com.raremarq",
      "id": "example-gallery",
      "url": "https://example.com/raremarq/u/example-raremarq-user/galleries/example-gallery"
    }
  ]
}
```

Raremarq piece identity maps to artwork-level `external_links`.

```json
{
  "id": "artwork-example-001",
  "title": "Example Artwork Title",
  "external_links": [
    {
      "provider": "com.raremarq",
      "id": "example-raremarq-piece",
      "url": "https://example.com/raremarq/u/example-raremarq-user/pieces/example-raremarq-piece"
    }
  ]
}
```

Raremarq source image metadata can be preserved on file-level external links.

```json
{
  "files": [
    {
      "id": "file-1",
      "relative_path": "example-artwork.png",
      "file_kind": "raw",
      "external_links": [
        {
          "provider": "com.raremarq",
          "id": "example-raremarq-image",
          "url": "https://example.com/raremarq/images/example-raremarq-image.jpg",
          "extensions": {
            "com.raremarq": {
              "image_index": 1
            }
          }
        }
      ]
    }
  ]
}
```

### Public Metadata, Listing Fields, And Draft Images

Raremarq public metadata can include primary-gallery hints, listing status, commerce fields, certification fields, and draft-image fields. User, gallery, piece, and source-image identity should be represented by `external_links` when the base external link fields are sufficient. Values already carried by base OAA title, description, artist-credit, media, or artwork-type fields should be derived from those base fields instead of being stored again in the Raremarq extension block.

```json
{
  "public_metadata": {
    "extensions": {
      "com.raremarq": {
        "primary_gallery_slug": "example-gallery",
        "piece_fields": {
          "piece[seller_selected_mature_rated]": "true",
          "piece[category_id]": "example-raremarq-category",
          "piece[highlight]": "true",
          "piece[accept_offers]": "true",
          "piece[min_offer_amount]": "500.00",
          "piece[for_sale_price]": "2500.00",
          "piece[stock_status]": "available",
          "piece[quantity]": "1",
          "piece[markets]": "primary-marketplace",
          "piece[shipping_cost]": "25.00",
          "piece[shipping_cost_intl]": "75.00",
          "piece[cgc_cert_id]": "example-cert-id"
        },
        "draft_image_fields": {
          "draft_ids": [
            "example-draft-image-1",
            "example-draft-image-2"
          ],
          "current_draft_id": "example-draft-image-1"
        }
      }
    }
  }
}
```

Raremarq `draft_image_fields` should be preserved only when they correspond to observed Raremarq state. A writer should not fabricate Raremarq draft IDs or image-host draft state.

## OA Curator

OA Curator application metadata belongs in `app.oa-curator` extension blocks. These fields are application implementation metadata and should not be treated as portable OAA semantics by other readers. Do not duplicate OAA manifest IDs, reference paths, or array order in `app.oa-curator` unless the application is preserving a distinct internal value that cannot be reconstructed from base OAA data.

### Collection, Gallery, And Artwork Bookkeeping

Application database identity, manifest bookkeeping, and app settings can be preserved in the collection `extensions.app.oa-curator` block.

```json
{
  "id": "collection-example",
  "name": "Example Collection",
  "extensions": {
    "app.oa-curator": {
      "collection_db_id": 1001,
      "artwork_id_label_preference": "application",
      "created_at": "2026-01-01T12:00:00Z",
      "updated_at": "2026-01-01T12:30:00Z"
    }
  }
}
```

Application gallery database identity and manifest bookkeeping can be preserved in the gallery `extensions.app.oa-curator` block.

```json
{
  "id": "gallery-example",
  "name": "Example Gallery",
  "extensions": {
    "app.oa-curator": {
      "gallery_db_id": 2001,
      "created_at": "2026-01-01T12:00:00Z",
      "updated_at": "2026-01-01T12:30:00Z"
    }
  }
}
```

Application artwork identity and source context can be preserved in the artwork `extensions.app.oa-curator` block.

```json
{
  "id": "artwork-example-001",
  "title": "Example Artwork Title",
  "extensions": {
    "app.oa-curator": {
      "artwork_db_id": 3001,
      "canonical_id": "app-artwork-001",
      "display_id": "APP-00001",
      "source_context": "Provider source metadata",
      "created_at": "2026-01-01T12:00:00Z",
      "updated_at": "2026-01-01T12:30:00Z"
    }
  }
}
```

### Reference Ordering And Membership

Collection gallery references should stay lightweight. The order of `galleries[]` already represents collection gallery order, so do not add an app extension block only to repeat that order. Mutable gallery metadata belongs in the `.oagallery` manifest.

```json
{
  "galleries": [
    {
      "id": "gallery-example",
      "path": "galleries/example-gallery/.oagallery"
    }
  ]
}
```

Collection artwork references are registry entries. The order of `artworks[]` already represents registry order, so do not add an app extension block only to repeat that order. Mutable artwork metadata belongs in the `.oaartwork` manifest.

```json
{
  "artworks": [
    {
      "id": "artwork-example-001",
      "path": "artworks/artwork-example-001/.oaartwork"
    }
  ]
}
```

Gallery artwork references represent membership and display order. The order of `artworks[]` already represents gallery display order. They should not duplicate mutable artwork metadata.

```json
{
  "artworks": [
    {
      "id": "artwork-example-001"
    }
  ]
}
```

### Artist Credits And Private Metadata

Artist credits should use base OAA fields. OA Curator readers and writers should translate internal role IDs to and from portable role values such as `Penciller` and `Inker`; do not add an `app.oa-curator` extension block only to echo the same role information.

```json
{
  "public_metadata": {
    "artist_credits": [
      {
        "display_name": "Example Artist",
        "first_name": "Example",
        "last_name": "Artist",
        "role": "Penciller"
      },
      {
        "display_name": "Example Inker",
        "first_name": "Example",
        "last_name": "Inker",
        "role": "Inker"
      }
    ]
  }
}
```

Private collection-management metadata should remain in base OAA private metadata fields. OA Curator should not add an `app.oa-curator` extension block merely to record that an internal private metadata row existed.

```json
{
  "private_metadata": {
    "purchase_price": "100.00 USD",
    "estimated_value": "150.00 USD",
    "purchase_date": "2026-01-01",
    "provenance": "Example provenance note.",
    "personal_notes": "Example private collector note."
  }
}
```

### File Assets And Derivatives

Application file assets map to base OAA artwork file objects. Local paths must not be emitted; use `relative_path` for the archive-embedded file.

```json
{
  "files": [
    {
      "id": "file-1",
      "relative_path": "example-artwork.png",
      "file_kind": "raw",
      "extensions": {
        "app.oa-curator": {
          "file_asset_id": 5001,
          "source_kind": "imported",
          "metadata_checked_at": "2026-01-01T12:05:00Z",
          "modified_at": "2026-01-01T12:04:00Z"
        }
      }
    }
  ]
}
```

OAA does not use a separate derivative object. Derived assets should be emitted as ordinary `files[]` entries with `file_kind: "derivative"` and application-specific derivative details in `extensions.app.oa-curator`.

```json
{
  "files": [
    {
      "id": "file-2",
      "relative_path": "example-artwork-caf-basic.png",
      "file_kind": "derivative",
      "extensions": {
        "app.oa-curator": {
          "derived_asset_id": 6001,
          "source_file_id": "file-1",
          "source_file_asset_id": 5001,
          "derivative_type": "png_export",
          "created_at": "2026-01-01T12:10:00Z"
        }
      }
    }
  ]
}
```

Common application `source_kind` values include `linked`, `copied`, `imported`, and `scanned`.

### Local Data That Should Not Be Emitted

Applications may store local operational fields that are useful inside the application but are not portable OAA metadata.

Do not emit these values into OAA manifests when they contain absolute local filesystem paths:

- `artwork.source_folder`
- `file_asset.original_path`
- `file_asset.current_path`
- `derived_asset.path`
- `file_operation_log.old_path`
- `file_operation_log.new_path`

If a writer wants to preserve file-operation history, it must first convert the history to archive-relative paths or logical file IDs. Otherwise, omit it.
