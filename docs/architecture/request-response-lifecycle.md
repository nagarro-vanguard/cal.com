# Request/Response Lifecycle

Cal.com's request/response lifecycle is built around Next.js's App Router and tRPC, providing both traditional HTTP API endpoints and type-safe procedure calls. The system handles multiple types of requests including web pages, API calls, webhooks, and real-time updates.

## Overview of Request Flow

```mermaid
sequenceDiagram
    participant Client
    participant CDN
    participant Middleware
    participant Router
    participant tRPC
    participant Business
    participant Database
    participant External

    Client->>CDN: HTTP Request
    CDN->>Middleware: Forward Request
    Middleware->>Middleware: Auth Check
    Middleware->>Middleware: Rate Limiting
    Middleware->>Router: Route Request

    alt Page Request
        Router->>Business: Server Component
        Business->>Database: Data Fetch
        Database-->>Business: Return Data
        Business-->>Router: Rendered Component
        Router-->>Client: HTML Response
    else API Request
        Router->>tRPC: Procedure Call
        tRPC->>Business: Execute Logic
        Business->>Database: Database Query
        Database-->>Business: Query Result
        Business->>External: External API Call
        External-->>Business: API Response
        Business-->>tRPC: Return Result
        tRPC-->>Router: JSON Response
        Router-->>Client: HTTP Response
    end
```

## Request Types and Routing

### 1. Page Requests (App Router)

```mermaid
graph TB
    subgraph "Next.js App Router"
        AppDir[app/ directory]
        Layout[layout.tsx]
        Page[page.tsx]
        Loading[loading.tsx]
        Error[error.tsx]
    end

    subgraph "Server Components"
        RSC[React Server Components]
        DataFetch[Server-side Data Fetching]
        Streaming[Streaming SSR]
    end

    subgraph "Client Components"
        Hydration[Client Hydration]
        Interactivity[Interactive Components]
        ClientState[Client State]
    end

    AppDir --> Layout
    Layout --> Page
    Page --> RSC
    RSC --> DataFetch
    DataFetch --> Streaming
    Streaming --> Hydration
    Hydration --> Interactivity
    Interactivity --> ClientState
```

**Flow Details:**
1. **Route Resolution**: Next.js matches URL to file system structure
2. **Layout Rendering**: Nested layouts provide consistent structure
3. **Server Components**: Data fetching and initial rendering on server
4. **Streaming**: Progressive page loading with React 18 Suspense
5. **Client Hydration**: JavaScript loads for interactivity

### 2. tRPC API Requests

```mermaid
graph TB
    subgraph "tRPC Request Flow"
        ClientCall[Client tRPC Call]
        Serialize[Serialize Request]
        HTTPPost[HTTP POST to /api/trpc]
        Middleware[tRPC Middleware]
        Procedure[Procedure Handler]
        Response[Serialize Response]
    end

    subgraph "tRPC Middleware Stack"
        Auth[Authentication]
        RBAC[Authorization/RBAC]
        Validate[Input Validation]
        Rate[Rate Limiting]
        Log[Logging]
    end

    ClientCall --> Serialize
    Serialize --> HTTPPost
    HTTPPost --> Middleware
    Middleware --> Auth
    Auth --> RBAC
    RBAC --> Validate
    Validate --> Rate
    Rate --> Log
    Log --> Procedure
    Procedure --> Response
    Response --> ClientCall
```

**tRPC Router Structure:**
```typescript
// packages/trpc/server/routers/
viewer/          # Authenticated user operations
├── me.ts       # User profile operations
├── teams.ts    # Team management
├── bookings.ts # Booking operations
└── ...

admin/          # Admin-only operations
├── users.ts    # User administration
├── features.ts # Feature toggles
└── ...

public/         # Public operations (no auth)
├── event.ts    # Public event data
├── booking.ts  # Public booking flow
└── ...
```

### 3. REST API Endpoints

Cal.com maintains REST endpoints for external integrations and webhooks.

```mermaid
graph LR
    subgraph "API v1 (Legacy)"
        V1Auth[/api/auth/*]
        V1Webhook[/api/webhook/*]
        V1Integration[/api/integrations/*]
    end

    subgraph "API v2 (Platform)"
        V2Auth[/api/v2/auth/*]
        V2Bookings[/api/v2/bookings/*]
        V2Events[/api/v2/event-types/*]
        V2Calendar[/api/v2/calendars/*]
    end

    subgraph "Internal APIs"
        TRPC[/api/trpc/*]
        Health[/api/health]
        Cron[/api/cron/*]
    end
```

## Middleware Chain

Cal.com uses Next.js middleware for cross-cutting concerns that need to run before route handlers.

```mermaid
graph TB
    Request[Incoming Request] --> Security[Security Headers]
    Security --> CORS[CORS Policy]
    CORS --> Auth[Authentication Check]
    Auth --> Org[Organization Context]
    Org --> Rate[Rate Limiting]
    Rate --> Rewrite[URL Rewriting]
    Rewrite --> Route[Route to Handler]

    subgraph "Middleware Functions"
        SecurityFunc[Security Middleware]
        CORSFunc[CORS Middleware]
        AuthFunc[Auth Middleware]
        OrgFunc[Organization Middleware]
        RateFunc[Rate Limit Middleware]
        RewriteFunc[Rewrite Middleware]
    end
```

**Middleware Implementation:**
```typescript
// apps/web/middleware.ts
export async function middleware(request: NextRequest) {
  // 1. Security headers
  const response = addSecurityHeaders(request);

  // 2. CORS handling
  if (isCorsRequest(request)) {
    return handleCors(request, response);
  }

  // 3. Authentication
  const session = await getSession(request);
  if (requiresAuth(request.url) && !session) {
    return redirectToLogin(request);
  }

  // 4. Organization context
  const orgSlug = extractOrgSlug(request);
  if (orgSlug) {
    response.headers.set('x-cal-org-slug', orgSlug);
  }

  // 5. Rate limiting
  const rateLimitResult = await checkRateLimit(request);
  if (rateLimitResult.blocked) {
    return new Response('Rate Limited', { status: 429 });
  }

  // 6. URL rewriting for multi-tenancy
  if (shouldRewrite(request)) {
    return rewriteUrl(request, response);
  }

  return response;
}
```

## Data Fetching Patterns

### Server-Side Data Fetching

```mermaid
graph TB
    subgraph "Server Components"
        PageComponent[Page Component]
        DirectDB[Direct DB Query via Prisma]
        tRPCServer[tRPC Server-side Call]
        Cache[Cache Layer]
    end

    subgraph "Data Sources"
        PostgreSQL[(PostgreSQL)]
        Redis[(Redis Cache)]
        External[External APIs]
    end

    PageComponent --> DirectDB
    PageComponent --> tRPCServer
    DirectDB --> PostgreSQL
    tRPCServer --> Cache
    Cache --> Redis
    Cache --> PostgreSQL
    tRPCServer --> External
```

### Client-Side Data Fetching

```mermaid
graph TB
    subgraph "Client Components"
        Component[React Component]
        tRPCClient[tRPC Client]
        TanStack[TanStack Query]
        OptUpdate[Optimistic Updates]
    end

    subgraph "State Management"
        ServerState[Server State Cache]
        ClientState[Client State]
        FormState[Form State]
    end

    Component --> tRPCClient
    tRPCClient --> TanStack
    TanStack --> ServerState
    Component --> OptUpdate
    OptUpdate --> ClientState
    Component --> FormState
```

## Authentication Flow

```mermaid
sequenceDiagram
    participant User
    participant Client
    participant NextAuth
    participant Provider
    participant Database
    participant Session

    User->>Client: Login Request
    Client->>NextAuth: Initiate OAuth
    NextAuth->>Provider: Redirect to Provider
    Provider->>User: Authentication Form
    User->>Provider: Credentials
    Provider->>NextAuth: Authorization Code
    NextAuth->>Provider: Exchange for Token
    Provider-->>NextAuth: Access Token
    NextAuth->>Database: Store/Update User
    Database-->>NextAuth: User Record
    NextAuth->>Session: Create Session
    Session-->>Client: Session Cookie
    Client-->>User: Authenticated State
```

## Error Handling

### Error Boundary Pattern

```mermaid
graph TB
    subgraph "Error Handling Layers"
        GlobalError[Global Error Boundary]
        PageError[Page Error Boundary]
        ComponentError[Component Error Boundary]
        APIError[API Error Handler]
    end

    subgraph "Error Types"
        ValidationError[Validation Errors]
        AuthError[Authentication Errors]
        DatabaseError[Database Errors]
        ExternalError[External API Errors]
        UnexpectedError[Unexpected Errors]
    end

    ValidationError --> ComponentError
    AuthError --> PageError
    DatabaseError --> APIError
    ExternalError --> APIError
    UnexpectedError --> GlobalError
```

### tRPC Error Handling

```typescript
// Error handling in tRPC procedures
export const bookingRouter = createTRPCRouter({
  create: protectedProcedure
    .input(createBookingSchema)
    .mutation(async ({ input, ctx }) => {
      try {
        // Validate business rules
        const validation = await validateBookingRequest(input);
        if (!validation.success) {
          throw new TRPCError({
            code: 'BAD_REQUEST',
            message: validation.error,
          });
        }

        // Create booking
        const booking = await ctx.prisma.booking.create({
          data: input,
        });

        return booking;
      } catch (error) {
        // Log error for monitoring
        logger.error('Booking creation failed', { error, input });

        // Re-throw with appropriate error code
        if (error instanceof TRPCError) {
          throw error;
        }

        throw new TRPCError({
          code: 'INTERNAL_SERVER_ERROR',
          message: 'Failed to create booking',
          cause: error,
        });
      }
    }),
});
```

## Caching Strategy

```mermaid
graph TB
    subgraph "Caching Layers"
        CDN[CDN Cache]
        NextCache[Next.js Cache]
        ServerCache[Server-side Cache]
        DatabaseCache[Database Cache]
        ClientCache[Client Cache]
    end

    subgraph "Cache Types"
        Static[Static Assets]
        Page[Page Cache]
        API[API Response Cache]
        Query[Database Query Cache]
        Session[Session Cache]
    end

    Static --> CDN
    Page --> NextCache
    API --> ServerCache
    Query --> DatabaseCache
    Session --> ClientCache
```

### Cache Implementation

```typescript
// Server-side caching with Redis
export async function getCachedUserBookings(userId: string) {
  const cacheKey = `user:${userId}:bookings`;

  // Try cache first
  const cached = await redis.get(cacheKey);
  if (cached) {
    return JSON.parse(cached);
  }

  // Fetch from database
  const bookings = await prisma.booking.findMany({
    where: { userId },
    include: { eventType: true },
  });

  // Cache for 5 minutes
  await redis.setex(cacheKey, 300, JSON.stringify(bookings));

  return bookings;
}

// Client-side caching with TanStack Query
export function useUserBookings() {
  return trpc.viewer.bookings.list.useQuery(undefined, {
    staleTime: 5 * 60 * 1000, // 5 minutes
    cacheTime: 10 * 60 * 1000, // 10 minutes
    refetchOnWindowFocus: false,
  });
}
```

## Real-time Updates

For features requiring real-time updates, Cal.com uses Server-Sent Events and WebSockets.

```mermaid
sequenceDiagram
    participant Client
    participant Server
    participant Database
    participant External

    Note over Client,External: Booking Status Updates

    Client->>Server: Subscribe to booking updates
    Server->>Client: SSE connection established

    External->>Server: Webhook: booking confirmed
    Server->>Database: Update booking status
    Database-->>Server: Status updated
    Server->>Client: SSE: booking confirmed

    Client->>Client: Update UI optimistically
```

## Performance Optimizations

### Request Optimization

1. **Connection Pooling**: Database connections managed by Prisma
2. **Query Optimization**: N+1 query prevention with Prisma includes
3. **Response Compression**: gzip/brotli compression
4. **CDN Integration**: Static assets served from CDN
5. **Edge Computing**: Some routes deployed to edge functions

### Monitoring and Observability

```mermaid
graph LR
    subgraph "Observability Stack"
        Metrics[Metrics Collection]
        Logging[Structured Logging]
        Tracing[Request Tracing]
        Alerts[Alert Management]
    end

    subgraph "Tools"
        Prometheus[Prometheus]
        Winston[Winston Logger]
        Sentry[Sentry]
        PagerDuty[PagerDuty]
    end

    Metrics --> Prometheus
    Logging --> Winston
    Tracing --> Sentry
    Alerts --> PagerDuty
```

## Best Practices

### Request Handling

1. **Validation**: Always validate input at API boundaries
2. **Authorization**: Check permissions for every protected operation
3. **Error Handling**: Provide meaningful error messages
4. **Rate Limiting**: Protect against abuse and DoS attacks
5. **Logging**: Log important events for debugging and monitoring

### Performance

1. **Minimize Database Queries**: Use efficient query patterns
2. **Cache Appropriately**: Cache expensive operations
3. **Optimize Bundle Size**: Code splitting and lazy loading
4. **Use Server Components**: Reduce client-side JavaScript
5. **Monitor Performance**: Track key metrics and optimize bottlenecks
