# Patreon GraphQL Schema

This directory contains a conceptual GraphQL schema for the Patreon platform, derived from
the [Patreon API v2](https://docs.patreon.com/) REST/JSON:API specification.

## Overview

Patreon is a membership platform for creators that provides tiered subscriptions, content
publishing, community features, and payment processing. The official API v2 follows the
JSON:API specification over REST and exposes resources including campaigns, members (patrons),
posts, tiers, benefits, deliverables, addresses, and webhooks via OAuth 2.0.

## Schema Source

- **Derived from:** Patreon API v2 REST documentation at https://docs.patreon.com/
- **Specification:** JSON:API v1.0 over REST (conceptually mapped to GraphQL)
- **Authentication:** OAuth 2.0 — creator access tokens and three-legged OAuth flows

## Types

### User and Identity

| Type | Description |
|------|-------------|
| `User` | Authenticated Patreon user (creator or patron) |
| `UserDetails` | Profile fields: name, image, about, vanity, social connections |
| `UserEmail` | Email address with verification and primary flags |
| `Creator` | A user acting as a content creator with campaigns |
| `CreatorDetails` | Creator-specific profile metadata |
| `CreatorType` | Label/value pair categorizing the creator's content type |

### Campaign

| Type | Description |
|------|-------------|
| `Campaign` | Root resource for a creator's membership campaign |
| `CampaignDetails` | Name, description, currency, pay-per-name, images |
| `CampaignStatus` | Enum: ACTIVE, INACTIVE, DRAFT |
| `CampaignUrl` | Canonical URL, vanity slug, creation name |

### Tiers

| Type | Description |
|------|-------------|
| `Tier` | A membership tier within a campaign |
| `TierDetails` | Amount in cents, currency, published state, free-tier flag |
| `TierAmount` | Enum: FREE or PAID |
| `TierTitle` | Wrapper holding the tier's display name |
| `TierDescription` | Plain text and HTML description of the tier |
| `TierPerks` | List of perks, welcome message, welcome message HTML |

### Patrons and Pledges

| Type | Description |
|------|-------------|
| `Patron` | A patron (member) relationship to a campaign |
| `PatronDetails` | Name, email, note, thumb URL, will-pay amount |
| `PatronStatus` | Enum: ACTIVE_PATRON, DECLINED_PATRON, FORMER_PATRON |
| `Pledge` | A patron's financial pledge to a campaign |
| `PledgeDetails` | Amount in cents, currency, cadence, status, cap |
| `PledgeAmount` | Cents and currency scalar pair |
| `PledgeCurrency` | Enum: USD, EUR, GBP, CAD, AUD |

### Membership

| Type | Description |
|------|-------------|
| `Membership` | Full member record linking user, campaign, tier, pledge, and deliverables |
| `MembershipDetails` | Email, name, note, patron status, entitlement amounts |
| `MembershipStatus` | Enum: ACTIVE, INACTIVE, PENDING, DECLINED, CANCELLED |
| `MembershipLifetimeSupport` | Lifetime support total in cents and currency |

### Goals

| Type | Description |
|------|-------------|
| `Goal` | A funding goal set on a campaign |
| `GoalDetails` | Title, description, HTML description |
| `GoalAmount` | Amount in cents, currency, completion percentage |
| `GoalStatus` | Enum: IN_PROGRESS, COMPLETE, OVER |

### Posts and Content

| Type | Description |
|------|-------------|
| `Post` | A piece of content published to a campaign |
| `PostDetails` | Title, content body, teaser, content type, public flag, minimum pledge |
| `PostType` | Enum: TEXT, IMAGE, VIDEO, AUDIO, LINK, POLL |
| `PostStatus` | Enum: PUBLISHED, DRAFT, SCHEDULED |
| `PostTier` | Tier gate applied to a post (id, title, amount) |
| `PostAccess` | Enum: PUBLIC, PATRONS_ONLY, TIER_LOCKED |

### Comments and Attachments

| Type | Description |
|------|-------------|
| `Comment` | A patron comment on a post |
| `CommentDetails` | Comment body text and deletion flag |
| `Attachment` | A file or media attachment on a post |
| `AttachmentDetails` | URL, name, media type, size in bytes |

### Benefits and Deliverables

| Type | Description |
|------|-------------|
| `Benefit` | A specific benefit offered within a tier |
| `BenefitDetails` | Title, description, type, published/ended/limit flags |
| `BenefitType` | Enum: DIGITAL_DOWNLOAD, CUSTOM, DISCORD_ROLE, PHYSICAL_GOOD, OTHER |
| `Deliverable` | An instance of a benefit owed to a specific member |
| `DeliverableDetails` | Status, due date, completion date |

### Webhooks

| Type | Description |
|------|-------------|
| `Webhook` | A configured webhook endpoint for campaign events |
| `WebhookEvent` | Enum of lifecycle events (pledge/subscription/post) |
| `WebhookTrigger` | Enum of trigger categories (CREATE, UPDATE, DELETE) |

### OAuth and Auth

| Type | Description |
|------|-------------|
| `OAuthClient` | A registered Patreon OAuth application |
| `OAuthToken` | Access/refresh token pair returned from OAuth flows |
| `Scope` | Enum of OAuth permission scopes |
| `APIKey` | A long-lived API key credential |
| `Token` | A token scoped to a specific permission |

### Address

| Type | Description |
|------|-------------|
| `Address` | Patron shipping or billing address |

## Queries

| Query | Description |
|-------|-------------|
| `me` | Authenticated user's identity |
| `campaign(id)` | Fetch a campaign by ID |
| `campaigns(cursor, pageSize)` | Paginated list of creator's campaigns |
| `member(id)` | A single membership record |
| `members(campaignId, cursor, pageSize)` | Paginated list of campaign members |
| `post(id)` | A single post |
| `posts(campaignId, cursor, pageSize)` | Paginated list of campaign posts |
| `tier(id)` | A single tier |
| `tiers(campaignId)` | All tiers for a campaign |
| `benefit(id)` | A single benefit |
| `webhooks(campaignId)` | All webhooks for a campaign |
| `address(id)` | A patron address record |

## Mutations

| Mutation | Description |
|----------|-------------|
| `createWebhook` | Register a new webhook for a campaign |
| `updateWebhook` | Change URI, triggers, or pause state |
| `deleteWebhook` | Remove a webhook |
| `updatePost` | Edit post content, status, or access level |

## Resources

- API Documentation: https://docs.patreon.com/
- OAuth Reference: https://docs.patreon.com/#oauth
- Webhooks Reference: https://docs.patreon.com/#webhooks
- Developer Portal: https://www.patreon.com/portal/registration/register-clients
- Developer Forums: https://www.patreondevelopers.com/
