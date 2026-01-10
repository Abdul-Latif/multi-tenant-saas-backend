# Learning Notes - Multi-Tenant SaaS Backend

This document tracks my learning journey, research findings, and evolving understanding of multi-tenant architecture.

## Understanding the Problem [i copied these notes from obsidian so it might look a little weird -also use obsedian- highly recommended for notes]

- SaaS (Software as a Service)
	- SaaS means software provided as a service, not a product you own.
	- You don’t buy the software, you pay for a license/subscription to use it.
	- You usually access it through the browser.
	- No need to:
	    - Install the software
	    - Update it
	    - Maintain servers
	    - Hire employees to manage it
	- The provider handles:
	    - Hosting
	    - Updates
	    - Monitoring
	    - Scaling
	- Example:
	    - Microsoft 365 → used online without installing anything locally.
-  Multi-Tenant SaaS – The Core Idea
	- A single application serves many users (tenants).
	- Each tenant:
	    - Has their own data
	    - Cannot see or access other tenants’ data
	- Example:
	    - Gmail
	        - Millions of users use the same system
	        - Each user only sees their own emails
	- A tenant = one customer (person, startup, or company).
-  The Problem Multi-Tenant SaaS Solves
	- One service must support:
	    - Individual users
	    - Small startups (hundreds or thousands of users)
	    - Large enterprises (millions of users)
	- All of them:
	    - Use the same app
	    - Expect privacy
	    - Expect good performance
	- The challenge:
	    - How to separate data
	    - How to scale
	    - How to keep costs reasonable
	    - How to avoid data leaks
-  Multi-Tenancy Data Architecture Options
	1. One Database per Tenant
		- Each tenant has their own database
		- Pros:
		    - Strong isolation
		    - Better security
		    - Easy data deletion (drop the DB)
		- Cons:
		    - Expensive
		    - Hard to maintain
		    - Hard migrations
		    - Analytics across tenants is a nightmare
	2. Single Shared Database (True Multi-Tenancy)
		- All tenants share one database
		- Pros:
		    - Cheaper
		    - Easier maintenance
		    - Easier schema migrations
			- Easier analytics
		- Cons:
		    - Requires very careful design
		    - One heavy tenant can affect others
			- One bug can expose another tenant’s data
		    - Data deletion must be handled carefully
	 How isolation is achieved:
	- Every request includes a `tenant_id`
	- Every table includes a `tenant_id`
	- Use row-level security (RLS) to enforce isolation
	- No query should ever return data without tenant filtering
	 3. Hybrid Approach (Most Common in Practice)
		- Start with:
		    - Single database
		    - Strong tenant isolation rules
		- As the system grows:
		    - Split large tenants into separate databases
		    - Or use separate schemas per tenant
		- Trade-offs:
		    - Similar pros/cons to multi-DB
		    - Slightly cheaper
		    - Still complex to manage at scale
-  Three main challenges
	- Challenge 1: Data Isolation
		-  1. Database per Tenant
			- How it works:  
			    Each tenant has a completely separate database. The application connects to a different database depending on 				which tenant is making the request.
			- Pros:
			    - Strongest level of isolation
			    - High security and compliance
			    - Easy to delete or migrate a tenant
			    - Bugs cannot leak data between tenants
			- Cons:
			    - High infrastructure cost
				- Hard to manage at scale (migrations, backups)
			    - Database connection limits become a problem
			    - Difficult to run analytics across all tenants
		- 2. Shared Database with Separate Schemas
			- How it works:  
			    All tenants share the same database server, but each tenant has its own schema containing its tables.
			- Pros:
			    - Good balance between isolation and cost
			    - Easier to customize per tenant
			    - Better security than fully shared tables
			- Cons:
			    - Schema migrations are more complex
			    - Still harder to manage at scale than a single schema
			    - More operational overhead than shared tables
		- 3.  Shared Database with Shared Schema
			- How it works:  
			    All tenants share the same database and the same tables. Each row includes a `tenant_id` column, and queries 				are filtered by this value.
			- Pros:
			    - Cheapest option
			    - Fast to implement
			    - Scales well for many tenants
			    - Easy to run analytics across tenants
			- Cons:
			    - Requires strict discipline in the codebase
			    - Bugs can cause data leakage if not handled properly
			    - Needs additional safeguards like Row-Level Security (RLS)
	- Challenge 2: Tenant Identification
		- subdomain routing
		- API key
		- JWT-based tenant identification
	- Challenge 3: Resource management
		- rate limiting per tenant
		- API gateway rate limiting
		- application level rate limiting
	- regarding points 2 and 3
		-` The tenant identifier is embedded in the JWT at authentication time and cryptographically signed by the server, preventing client-side tampering. Each request validates the JWT and extracts the tenant identity, which is used both for authorization and for enforcing rate limits on a per-tenant basis. Resource usage is controlled by limiting the number of requests a tenant can make within a defined time window, and any request exceeding this limit is rejected with an HTTP 429 response before reaching application or database layers. Database-level Row Level Security ensures that even if application logic fails, cross-tenant data access is impossible.` 
	- i will be using:
		- - Shared database with shared schema + RLS
		- JWT-based tenant identification
		- Application-level rate limiting

## Questions & Answers

[i will be adding the questions that will come up later, also inner questions if i ever have ones]
