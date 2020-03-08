# Migration `20200308165423-init`

This migration has been generated by isaac at 3/8/2020, 4:54:23 PM.
You can check out the [state of the schema](./schema.prisma) after the migration.

## Database Steps

```sql
CREATE TABLE "public"."User" (
    "bio" text  NOT NULL DEFAULT '',
    "createdAt" timestamp(3)  NOT NULL DEFAULT '1970-01-01 00:00:00',
    "displayName" text   ,
    "dob" text  NOT NULL DEFAULT '',
    "email" text  NOT NULL DEFAULT '',
    "id" SERIAL,
    "location" text  NOT NULL DEFAULT '',
    "name" text   ,
    "phone" text  NOT NULL DEFAULT '',
    "updatedAt" timestamp(3)  NOT NULL DEFAULT '1970-01-01 00:00:00',
    PRIMARY KEY ("id")
) 

CREATE TABLE "public"."Post" (
    "author" integer   ,
    "content" text   ,
    "createdAt" timestamp(3)  NOT NULL DEFAULT '1970-01-01 00:00:00',
    "id" SERIAL,
    "mediaURL" text  NOT NULL DEFAULT '',
    "published" boolean  NOT NULL DEFAULT false,
    "updatedAt" timestamp(3)  NOT NULL DEFAULT '1970-01-01 00:00:00',
    PRIMARY KEY ("id")
) 

CREATE TABLE "public"."_User" (
    "A" integer  NOT NULL ,
    "B" integer  NOT NULL 
) 

CREATE UNIQUE INDEX "User.email" ON "public"."User"("email")

CREATE UNIQUE INDEX "_User_AB_unique" ON "public"."_User"("A","B")

ALTER TABLE "public"."Post" ADD FOREIGN KEY ("author")REFERENCES "public"."User"("id") ON DELETE SET NULL  ON UPDATE CASCADE

ALTER TABLE "public"."_User" ADD FOREIGN KEY ("A")REFERENCES "public"."User"("id") ON DELETE CASCADE  ON UPDATE CASCADE

ALTER TABLE "public"."_User" ADD FOREIGN KEY ("B")REFERENCES "public"."User"("id") ON DELETE CASCADE  ON UPDATE CASCADE
```

## Changes

```diff
diff --git schema.prisma schema.prisma
migration 20200308160538-init..20200308165423-init
--- datamodel.dml
+++ datamodel.dml
@@ -1,23 +1,36 @@
 datasource db {
-  provider = "sqlite"
-  url = "***"
+  provider = "postgresql"
+  url      = "postgresql://postgres:docker@192.168.99.100:5432/pg-docker?schema=public"
 }
 generator client {
   provider = "prisma-client-js"
 }
+model User {
+  id    Int     @id @default(autoincrement())
+  createdAt DateTime @default(now())
+  updatedAt DateTime @updatedAt
+  name String?
+  displayName String?
+  email String  @unique
+  dob String
+  phone String
+  location String
+  bio String
+  posts Post[]
+  followers User[] @relation(name: "User")
+  following User[] @relation(name: "User")
+}
+
 model Post {
   id        Int     @id @default(autoincrement())
-  title     String
+  createdAt DateTime @default(now())
+  updatedAt DateTime @updatedAt
+  author    User?
   content   String?
+  mediaURL String
   published Boolean @default(false)
-  author    User?
+  
 }
-model User {
-  id    Int     @id @default(autoincrement())
-  email String  @unique
-  name  String?
-  posts Post[]
-}
```

