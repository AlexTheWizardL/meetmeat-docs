# MeetMeAt - Technical Specification

> Data models, API contracts, environment setup, and deployment.

---

## 1. Data Models

### Base Entity

All entities extend this (UUID keys, timestamps, soft delete, optimistic locking):

```typescript
// src/common/entities/base.entity.ts
import {
  PrimaryGeneratedColumn,
  CreateDateColumn,
  UpdateDateColumn,
  DeleteDateColumn,
  VersionColumn,
} from 'typeorm';

export abstract class BaseEntity {
  @PrimaryGeneratedColumn('uuid')
  id: string;

  @CreateDateColumn({ name: 'created_at' })
  createdAt: Date;

  @UpdateDateColumn({ name: 'updated_at' })
  updatedAt: Date;

  @DeleteDateColumn({ name: 'deleted_at' })
  deletedAt?: Date;

  @VersionColumn()
  version: number;
}
```

---

### Profile

User profiles (local-first, no auth for MVP).

```typescript
// src/modules/profiles/entities/profile.entity.ts
@Entity('profiles')
export class Profile extends BaseEntity {
  @Column({ type: 'varchar', length: 255 })
  name: string;

  @Column({ type: 'varchar', length: 255 })
  title: string;

  @Column({ type: 'varchar', length: 255, nullable: true })
  company?: string;

  @Column({ type: 'text', nullable: true })
  avatarUrl?: string;

  @Column({ type: 'jsonb', default: () => "'[]'" })
  socialLinks: Array<{
    platform: 'linkedin' | 'twitter' | 'instagram' | 'facebook' | 'website';
    url: string;
  }>;

  @Column({ type: 'boolean', default: false })
  isDefault: boolean;

  @OneToMany(() => Poster, (poster) => poster.profile)
  posters: Poster[];
}
```

---

### Event

Cached event metadata from URL parsing.

```typescript
// src/modules/events/entities/event.entity.ts
@Entity('events')
export class Event extends BaseEntity {
  @Column({ type: 'text', unique: true })
  sourceUrl: string;

  @Column({ type: 'varchar', length: 255 })
  name: string;

  @Column({ type: 'text', nullable: true })
  description?: string;

  @Column({ type: 'date', nullable: true })
  startDate?: Date;

  @Column({ type: 'date', nullable: true })
  endDate?: Date;

  @Column({ type: 'jsonb', nullable: true })
  location?: {
    venue?: string;
    city?: string;
    country?: string;
    isVirtual: boolean;
  };

  @Column({ type: 'text', nullable: true })
  logoUrl?: string;

  @Column({ type: 'jsonb', nullable: true })
  brandColors?: {
    primary: string;
    secondary?: string;
  };

  @Column({ type: 'jsonb', default: () => "'{}'" })
  rawMetadata: Record<string, any>;

  @OneToMany(() => Poster, (poster) => poster.event)
  posters: Poster[];
}
```

---

### Template

AI-generated poster templates.

```typescript
// src/modules/templates/entities/template.entity.ts
@Entity('templates')
export class Template extends BaseEntity {
  @Column({ type: 'varchar', length: 255 })
  name: string;

  @Column({ type: 'text' })
  previewImageUrl: string;

  @Column({ type: 'jsonb' })
  design: {
    layout: 'classic' | 'modern' | 'minimal' | 'bold';
    backgroundColor: string;
    elements: Array<{
      id: string;
      type: 'text' | 'image' | 'shape' | 'logo';
      properties: {
        x: number;
        y: number;
        width: number;
        height: number;
        content?: string;
        fill?: string;
        fontSize?: number;
        fontFamily?: string;
        [key: string]: any;
      };
    }>;
  };

  @Column({ type: 'varchar', length: 50, default: 'active' })
  status: 'active' | 'archived';

  @Column({ type: 'integer', default: 0 })
  usageCount: number;

  @OneToMany(() => Poster, (poster) => poster.template)
  posters: Poster[];
}
```

---

### Poster

User-created posters with customizations.

```typescript
// src/modules/posters/entities/poster.entity.ts
@Entity('posters')
export class Poster extends BaseEntity {
  @Column({ type: 'uuid' })
  profileId: string;

  @Column({ type: 'uuid', nullable: true })
  eventId?: string;

  @Column({ type: 'uuid', nullable: true })
  templateId?: string;

  @ManyToOne(() => Profile, { onDelete: 'CASCADE' })
  @JoinColumn({ name: 'profile_id' })
  profile: Profile;

  @ManyToOne(() => Event, { onDelete: 'SET NULL' })
  @JoinColumn({ name: 'event_id' })
  event?: Event;

  @ManyToOne(() => Template, { onDelete: 'SET NULL' })
  @JoinColumn({ name: 'template_id' })
  template?: Template;

  // Manual event entry (fallback when URL parsing fails)
  @Column({ type: 'varchar', length: 255, nullable: true })
  manualEventName?: string;

  @Column({ type: 'date', nullable: true })
  manualEventDate?: Date;

  @Column({ type: 'varchar', length: 255, nullable: true })
  manualEventLocation?: string;

  @Column({ type: 'varchar', length: 10, nullable: true })
  manualBrandColor?: string;

  // Customizations (overrides template defaults)
  @Column({ type: 'jsonb', default: () => "'{}'" })
  customizations: {
    backgroundColor?: string;
    textColor?: string;
    elementOverrides?: Record<string, any>;
  };

  // Export history
  @Column({ type: 'jsonb', default: () => "'[]'" })
  exports: Array<{
    platform: 'linkedin' | 'instagram' | 'twitter' | 'facebook';
    width: number;
    height: number;
    imageUrl: string;
    exportedAt: Date;
  }>;

  @Column({ type: 'varchar', length: 50, default: 'draft' })
  status: 'draft' | 'completed' | 'archived';

  @Column({ type: 'text', nullable: true })
  thumbnailUrl?: string;
}
```

---

### Entity Relationships

```
Profile ──┐
          │
Event ────┼──► Poster
          │
Template ─┘

- Profile 1:N Poster (required, CASCADE delete)
- Event 1:N Poster (optional, SET NULL on delete)
- Template 1:N Poster (optional, SET NULL on delete)
```

---

## 2. API Endpoints

**Base:** `/api/v1`

### Profiles

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/profiles` | List all |
| `GET` | `/profiles/:id` | Get one |
| `POST` | `/profiles` | Create |
| `PATCH` | `/profiles/:id` | Update |
| `DELETE` | `/profiles/:id` | Soft delete |
| `PATCH` | `/profiles/:id/default` | Set as default |

### Events

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/events/parse` | Parse URL with AI |
| `GET` | `/events/:id` | Get cached event |
| `POST` | `/events` | Create manual event |

### Templates

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/templates/generate` | Generate for event |
| `GET` | `/templates` | List available |
| `GET` | `/templates/:id` | Get details |

### Posters

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/posters` | List (filterable) |
| `GET` | `/posters/:id` | Get details |
| `POST` | `/posters` | Create |
| `PATCH` | `/posters/:id` | Update customizations |
| `POST` | `/posters/:id/export` | Export to platform |
| `DELETE` | `/posters/:id` | Soft delete |

### Health

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/health` | Service status |

---

## 3. Environment Setup

### Files

```
.env              # Defaults (committed)
.env.local        # Secrets (gitignored)
.env.test         # Test environment
```

### Variables

**.env (committed):**
```bash
NODE_ENV=development
PORT=3000

# Database (local Docker defaults)
DATABASE_HOST=localhost
DATABASE_PORT=5432
DATABASE_USER=postgres
DATABASE_NAME=meetmeat

# AI (swappable)
AI_PROVIDER=openai

# Storage
STORAGE_PROVIDER=local

# CORS
CORS_ORIGIN=http://localhost:19006
```

**.env.local (gitignored):**
```bash
DATABASE_PASSWORD=postgres
OPENAI_API_KEY=sk-...
ANTHROPIC_API_KEY=sk-ant-...
AWS_ACCESS_KEY_ID=...
AWS_SECRET_ACCESS_KEY=...
AWS_S3_BUCKET=meetmeat-uploads
```

### Validation

```typescript
// src/config/validation.ts
import * as Joi from 'joi';

export const validationSchema = Joi.object({
  NODE_ENV: Joi.string().valid('development', 'staging', 'production', 'test').default('development'),
  PORT: Joi.number().default(3000),
  DATABASE_HOST: Joi.string().required(),
  DATABASE_PORT: Joi.number().default(5432),
  DATABASE_USER: Joi.string().required(),
  DATABASE_PASSWORD: Joi.string().required(),
  DATABASE_NAME: Joi.string().required(),
  AI_PROVIDER: Joi.string().valid('openai', 'anthropic').default('openai'),
  OPENAI_API_KEY: Joi.string().when('AI_PROVIDER', { is: 'openai', then: Joi.required() }),
  ANTHROPIC_API_KEY: Joi.string().when('AI_PROVIDER', { is: 'anthropic', then: Joi.required() }),
  STORAGE_PROVIDER: Joi.string().valid('local', 's3').default('local'),
  CORS_ORIGIN: Joi.string().default('*'),
});
```

---

## 4. Local Development

### Docker Compose

```yaml
# docker-compose.yml
version: '3.8'

services:
  postgres:
    image: postgres:16-alpine
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: meetmeat
    ports:
      - '5432:5432'
    volumes:
      - postgres_data:/var/lib/postgresql/data
    healthcheck:
      test: ['CMD-SHELL', 'pg_isready -U postgres']
      interval: 10s
      timeout: 5s
      retries: 5

  api:
    build:
      context: .
      target: development
    command: npm run start:dev
    ports:
      - '3000:3000'
    environment:
      DATABASE_HOST: postgres
      DATABASE_PORT: 5432
      DATABASE_USER: postgres
      DATABASE_PASSWORD: postgres
      DATABASE_NAME: meetmeat
    env_file:
      - .env.local
    volumes:
      - .:/usr/src/app
      - /usr/src/app/node_modules
    depends_on:
      postgres:
        condition: service_healthy

volumes:
  postgres_data:
```

### Commands

```bash
docker compose up -d      # Start
docker compose logs -f    # Logs
docker compose down       # Stop
docker compose down -v    # Reset DB
```

### Dockerfile

```dockerfile
FROM node:20-alpine AS development
RUN apk add --no-cache dumb-init
WORKDIR /usr/src/app
COPY package*.json ./
RUN npm ci
COPY . .
USER node

FROM node:20-alpine AS build
WORKDIR /usr/src/app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build
RUN npm ci --only=production && npm cache clean --force

FROM node:20-alpine AS production
RUN apk add --no-cache dumb-init
WORKDIR /usr/src/app
COPY --from=build /usr/src/app/node_modules ./node_modules
COPY --from=build /usr/src/app/dist ./dist
RUN addgroup -g 1001 -S nodejs && adduser -S nestjs -u 1001
USER nestjs
EXPOSE 3000
CMD ["dumb-init", "node", "dist/main"]
```

---

## 5. Deployment (Railway)

### Why Railway
- Auto-detects NestJS, zero config
- Built-in PostgreSQL
- Preview environments for PRs
- $5-20/month

### Steps

1. Push to GitHub
2. railway.app → New Project → Deploy from GitHub
3. Add PostgreSQL service
4. Set environment variables in dashboard:
   ```
   NODE_ENV=production
   AI_PROVIDER=openai
   OPENAI_API_KEY=sk-...
   CORS_ORIGIN=https://your-frontend.app
   STORAGE_PROVIDER=s3
   AWS_ACCESS_KEY_ID=...
   AWS_SECRET_ACCESS_KEY=...
   AWS_S3_BUCKET=meetmeat-uploads
   ```
5. Enable PR preview deploys

### CI/CD

```yaml
# .github/workflows/ci.yml
name: CI
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    services:
      postgres:
        image: postgres:16-alpine
        env:
          POSTGRES_USER: postgres
          POSTGRES_PASSWORD: postgres
          POSTGRES_DB: test
        ports:
          - 5432:5432
        options: --health-cmd pg_isready --health-interval 10s --health-timeout 5s --health-retries 5
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
      - run: npm ci
      - run: npm run lint
      - run: npm run test
        env:
          DATABASE_URL: postgresql://postgres:postgres@localhost:5432/test
```

---

## 6. Project Structure

```
src/
├── common/
│   ├── entities/base.entity.ts
│   ├── filters/
│   └── interceptors/
├── config/
│   ├── database.config.ts
│   ├── ai.config.ts
│   └── validation.ts
├── modules/
│   ├── profiles/
│   │   ├── dto/
│   │   ├── entities/profile.entity.ts
│   │   ├── profiles.controller.ts
│   │   ├── profiles.service.ts
│   │   └── profiles.module.ts
│   ├── events/
│   ├── templates/
│   ├── posters/
│   ├── ai/
│   │   ├── providers/openai.provider.ts
│   │   ├── providers/anthropic.provider.ts
│   │   └── ai.service.ts
│   └── storage/
│       ├── providers/local.provider.ts
│       ├── providers/s3.provider.ts
│       └── storage.service.ts
├── database/
│   └── migrations/
├── app.module.ts
└── main.ts
```

---

## 7. Migrations

**Never use `synchronize: true` in production.**

```bash
# Generate from entity changes
npm run migration:generate -- src/database/migrations/AddField

# Run migrations
npm run migration:run

# Revert last migration
npm run migration:revert
```

---

## Export Sizes

| Platform | Dimensions |
|----------|------------|
| LinkedIn | 1200×627 |
| Instagram | 1080×1080 |
| Twitter/X | 1200×675 |
| Facebook | 1200×630 |

---

*Last Updated: December 2024*
