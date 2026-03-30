---
name: multi-tenant
description: Work with Django Sites framework, SiteAwareModel, and site isolation. Use when creating/modifying models, working with multi-tenancy, or when site context is involved.
allowed-tools: Read, Grep, Glob
---

# Multi-Tenancy (Site Isolation)

Uses Django Sites framework for automatic site isolation.

## When to Use This Skill

Use this Skill when:
- **Creating new models** - Need to determine if they should be site-aware
- **Modifying existing models** - Working with SiteAwareModel or SiteAwareModelBase
- **Debugging site isolation** - Issues with cross-site data leakage
- **User mentions "site", "multi-tenant", "SiteAwareModel"**
- **Writing queries** - Need to understand automatic site filtering
- **Working with the admin** - Site-aware model admin configuration

## Core Components

- **SiteAwareModelBase** - Adds `site` ForeignKey + SiteAwareManager
- **SiteAwareModel** - Extends SiteAwareModelBase with UUID primary key
- **CurrentSiteMiddleware** - Stores current request in thread-local (`_thread_locals.request`)

## How It Works

1. Manager's `get_queryset()` automatically filters by current site
2. On save, models auto-populate `site_id` from thread-local request
3. Site determined by request domain

**Result:** All queries automatically scope to current site.

## Key Rules

- Extend `SiteAwareModel` for models that need site isolation (includes UUID pk + site FK)
- Extend `SiteAwareModelBase` if you need site isolation but a custom primary key
- Manager's `get_queryset()` automatically filters by current site — no manual filtering needed
- On save, `site_id` auto-populates from the current request — never set it manually
- In tests, always use `mock_site_context` fixture

## Key Files

- `freedom_ls/site_aware_models/models.py`
- `freedom_ls/site_aware_models/middleware.py`
- `freedom_ls/site_aware_models/admin.py`
