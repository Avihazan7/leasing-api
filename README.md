# leasing-api
פלטפורמה מסחר שיווק ומכירת רכבים חדשים 

{
  "name": "leasing-deal-score-engine",
  "version": "1.0.0",
  "description": "Leasing.co.il Deal Score Engine API",
  "main": "api/index.js",
  "scripts": {
    "dev": "vercel dev",
    "build": "tsc",
    "test": "vitest run"
  },
  "dependencies": {
    "@supabase/supabase-js": "^2.39.3",
    "cors": "^2.8.5",
    "dotenv": "^16.4.5",
    "express": "^4.18.2",
    "helmet": "^7.1.0",
    "zod": "^3.22.4"
  },
  "devDependencies": {
    "@types/cors": "^2.8.17",
    "@types/express": "^4.17.21",
    "@types/node": "^20.11.24",
    "@types/supertest": "^6.0.2",
    "supertest": "^6.3.4",
    "typescript": "^5.3.3",
    "vitest": "^1.3.1"
  }
}{
  "compilerOptions": {
    "target": "ES2022",
    "module": "CommonJS",
    "moduleResolution": "node",
    "outDir": "./dist",
    "rootDir": ".",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "resolveJsonModule": true
  },
  "include": ["src/**/*", "api/**/*"],
  "exclude": ["node_modules", "**/*.test.ts"]
}{
  "version": 2,
  "builds": [
    {
      "src": "api/index.ts",
      "use": "@vercel/node"
    }
  ],
  "routes": [
    {
      "src": "/(.*)",
      "dest": "api/index.ts"
    }
  ]
}/**
 * Vercel Serverless Entrypoint
 */
import app from '../src/server';

export default app;import { createClient } from '@supabase/supabase-js';
import dotenv from 'dotenv';

dotenv.config();

const supabaseUrl = process.env.SUPABASE_URL || '';
const supabaseServiceKey = process.env.SUPABASE_SERVICE_ROLE_KEY || '';

if (!supabaseUrl || !supabaseServiceKey) {
  console.warn('Missing SUPABASE_URL or SUPABASE_SERVICE_ROLE_KEY environment variables.');
}

// Using Service Role key to bypass RLS for server-side logic
export const supabase = createClient(supabaseUrl, supabaseServiceKey, {
  auth: {
    persistSession: false,
    autoRefreshToken: false,
  }
});/**
 * Basic logger to satisfy the existing code structure.
 * Can be easily swapped with Winston or Pino later.
 */
export const logger = {
    info: (msg: string, meta?: any) => console.log(`[INFO] ${msg}`, meta ? JSON.stringify(meta) : ''),
    warn: (msg: string, meta?: any) => console.warn(`[WARN] ${msg}`, meta ? JSON.stringify(meta) : ''),
    error: (msg: string, meta?: any) => console.error(`[ERROR] ${msg}`, meta ? JSON.stringify(meta) : ''),
    debug: (msg: string, meta?: any) => console.debug(`[DEBUG] ${msg}`, meta ? JSON.stringify(meta) : ''),
  };├── api/
│   └── index.ts
├── src/
│   ├── db/
│   │   └── supabase.ts
│   ├── lib/
│   │   └── logger.ts
│   ├── middleware/
│   │   └── hmacAuth.ts      <-- הקוד שלך
│   ├── routes/
│   │   └── api.ts           <-- הקוד שלך
│   ├── scoring/
│   │   └── dealScore.ts     <-- הקוד שלך
│   ├── schemas.ts           <-- הקוד שלך
│   └── server.ts
├── package.json
├── tsconfig.json
└── vercel.json