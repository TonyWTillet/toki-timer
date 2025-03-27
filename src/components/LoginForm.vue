<script setup lang="ts">
import { Button } from '@/components/ui/button'
import {
  Card,
  CardContent,
  CardDescription,
  CardHeader,
  CardTitle,
} from '@/components/ui/card'
import { Input } from '@/components/ui/input'
import { Label } from '@/components/ui/label'
import { ArrowLeftIcon } from '@heroicons/vue/24/outline'

const props = defineProps({
  firstName: {
    type: String,
    required: true
  },
})

const emit = defineEmits<{
  (e: 'update:step', value: number): void
  (e: 'end'): void
  (e: 'update:firstName', value: string): void
}>()

const handlePrevious = () => {
  emit('update:step', 0)
  emit('update:firstName', '')
}

const handleSkip = () => {
  emit('end')
}

</script>

<template>
  <Card class="mx-auto max-w-3xl">
    <CardHeader>
      <button @click="handlePrevious" class="text-gray-400 text-sm flex items-center gap-x-2 mb-4">
          <ArrowLeftIcon class="w-5 h-5" /> 
          <span class="text-gray-400 text-sm">Previous</span>
      </button>
      <CardTitle class="text-2xl">
        <h2 class="text-4xl text-gray-800 font-extrabold mx-auto md:text-5xl mb-4">
                Hi {{ firstName }}, <br> let's get you started
              </h2>
      </CardTitle>
      <CardDescription>
        Enter your email below to login to your account
      </CardDescription>
    </CardHeader>
    <CardContent>
      <div class="grid gap-4">
        <div class="grid gap-2">
          <Label for="email">Email</Label>
          <Input
            id="email"
            type="email"
            placeholder="m@example.com"
            required
          />
        </div>
        <div class="grid gap-2">
          <div class="flex items-center">
            <Label for="password">Password</Label>
            <a href="#" class="ml-auto inline-block text-sm underline">
              Forgot your password?
            </a>
          </div>
          <Input id="password" type="password" required />
        </div>
        <Button type="submit" class="w-full">
          Login
        </Button>
        <Button variant="outline" class="w-full">
          Login with Google
        </Button>
      </div>
      <div class="mt-4 text-center text-sm">
        Don't have an account?
        <a href="#" class="underline">
          Sign up
        </a>
      </div>
      <div class="mt-4 text-center text-sm">
        <button @click="handleSkip" class="underline">
          Skip and continue
        </button>
      </div>
    </CardContent>
  </Card>
</template>
