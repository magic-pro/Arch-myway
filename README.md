# 🏡 Arch-myway

> A gRPC-first home layout and interior design suggestion platform — built with Protobuf, Buf, and Connect.

-----

## What Is This?

**Arch-myway** lets users design their home layout (1-story, 2-story, or split-level) and receive AI-powered design suggestions for every room — living room, bedroom, study, kitchen, and more.

The API is defined entirely in **Protobuf** using a schema-first approach. This means:

- The `.proto` files are the **single source of truth**
- Client SDKs in Go, TypeScript, and Swift are **generated automatically**
- Breaking changes are **caught before they ship** via CI
- The API speaks **gRPC, gRPC-Web, and Connect Protocol** natively

-----

## Project Structure

```
Arch-myway/
├── proto/
│   └── homelayout/
│       └── v1/
│           ├── house.proto          ← House & architectural models
│           ├── room.proto           ← Room definitions & preferences
│           ├── suggestions.proto    ← Design suggestion models
│           └── service.proto        ← gRPC service definition
├── gen/
│   ├── go/                          ← Generated Go server/client code
│   ├── ts/                          ← Generated TypeScript/JS code
│   └── swift/                       ← Generated Swift (iOS) code
├── .github/
│   └── workflows/
│       └── buf-ci.yml               ← CI: lint + breaking change detection
├── buf.yaml                         ← Buf module config
├── buf.gen.yaml                     ← Code generation config
├── .gitignore
└── README.md
```

-----

## API Overview

### Service: `HouseLayoutService`

|RPC                    |Type            |Description                           |
|-----------------------|----------------|--------------------------------------|
|`CreateHouse`          |Unary           |Save a new house blueprint            |
|`GetHouse`             |Unary           |Retrieve a saved house                |
|`UpdateRoomPreferences`|Unary           |Update preferences for a specific room|
|`GetDesignSuggestions` |Unary           |Get design suggestions for any room   |
|`StreamDesignIdeas`    |Server streaming|Stream AI design ideas in real-time   |
|`AnalyzeRoomPhotos`    |Client streaming|Upload room photos for style analysis |

### Key Data Models

|Message               |Purpose                                                 |
|----------------------|--------------------------------------------------------|
|`House`               |Full house blueprint (stories, style, rooms, budget)    |
|`Room`                |Individual room with dimensions & preferences           |
|`RoomPreferences`     |Color palette, flooring, lighting, furniture style      |
|`DesignSuggestion`    |Design idea with furniture, decor, colors, lighting tips|
|`LivingRoomSuggestion`|Living room specific — sofa layout, entertainment setup |
|`BedroomSuggestion`   |Bedroom specific — bed size, placement, ambiance        |
|`StudySuggestion`     |Study specific — desk type, storage, reading nook       |

-----

## Getting Started

### Prerequisites

- [Git](https://git-scm.com/)
- [Buf CLI](https://buf.build/docs/installation) — see installation below
- [Go 1.22+](https://golang.org/dl/) *(for Go server/client)*
- [Node.js 18+](https://nodejs.org/) *(for TypeScript client)*

-----

### Step 1: Install Buf CLI

**macOS (Homebrew):**

```bash
brew install bufbuild/buf/buf
```

**Linux / WSL:**

```bash
curl -sSL \
  "https://github.com/bufbuild/buf/releases/latest/download/buf-Linux-x86_64" \
  -o /usr/local/bin/buf
chmod +x /usr/local/bin/buf
```

**Windows (via Scoop):**

```bash
scoop install buf
```

**Verify installation:**

```bash
buf --version
```

-----

### Step 2: Clone the Repo

```bash
git clone https://github.com/magic-pro/Arch-myway.git
cd Arch-myway
```

-----

### Step 3: Lint the Proto Files

```bash
buf lint
```

Expected output: ✅ no output means all schemas are clean.

-----

### Step 4: Check for Breaking Changes

```bash
buf breaking --against "https://github.com/magic-pro/Arch-myway.git#branch=main"
```

This compares your local changes against `main` and catches any API-breaking changes before you push.

-----

### Step 5: Generate Client & Server Code

```bash
buf generate
```

This will populate:

- `gen/go/` — Go server handlers and clients (via connect-go)
- `gen/ts/` — TypeScript clients (via connect-es)

-----

## CI/CD — Shift-Left API Quality

Every pull request automatically runs:

|Check         |What It Does                                   |
|--------------|-----------------------------------------------|
|`buf lint`    |Enforces Protobuf style & naming conventions   |
|`buf breaking`|Detects breaking API changes vs `main`         |
|`buf generate`|Verifies generated code is in sync with schemas|

This means **bad API contracts never reach production** — they’re caught at PR time. This is shift-left testing applied to your API layer.

-----

## Supported Protocols

Thanks to [Connect](https://connectrpc.com/), the server speaks all three protocols out of the box:

|Protocol            |Use Case                                                    |
|--------------------|------------------------------------------------------------|
|**Connect Protocol**|Modern browsers & mobile apps (JSON over HTTP/1.1 or HTTP/2)|
|**gRPC**            |Server-to-server communication                              |
|**gRPC-Web**        |Browsers that need gRPC without a proxy                     |

-----

## Roadmap

- [ ] Go server implementation
- [ ] TypeScript React frontend (room builder UI)
- [ ] AI design suggestion engine integration
- [ ] Room photo upload & style detection
- [ ] Buf Schema Registry (BSR) publishing
- [ ] Swift iOS client

-----

## Contributing

1. Fork the repo
1. Create a feature branch: `git checkout -b feat/my-feature`
1. Make your changes to `.proto` files
1. Run `buf lint` and `buf breaking` locally before pushing
1. Open a Pull Request — CI will validate your schemas automatically

-----

## License

MIT © [magic-pro](https://github.com/magic-pro)
