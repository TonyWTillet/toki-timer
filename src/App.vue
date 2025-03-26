<script setup lang="ts">
import { Button } from '@/components/ui/button'
import {
  Card,
  CardContent,
  CardDescription,
  CardFooter,
  CardHeader,
  CardTitle,
} from '@/components/ui/card'
import { AspectRatio } from '@/components/ui/aspect-ratio'

</script>

<template>
  <main class="bg-zinc-950 dark:bg-white">
    <div class="flex flex-col items-center justify-center h-screen">
      <Card>
        <CardHeader>
          <CardTitle><h1>TokiTimer, a timer for studying</h1></CardTitle>
          <CardDescription></CardDescription>
        </CardHeader>
        <CardContent>
          Card Content
        </CardContent>
        <CardFooter>
          Card Footer
        </CardFooter>
      </Card>
    </div>
  </main>
</template>
