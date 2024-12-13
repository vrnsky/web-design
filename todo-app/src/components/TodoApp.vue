<script setup lang="ts">
import { ref } from "vue";
import { Button } from "@/components/ui/button";
import { Input } from "@/components/ui/input";
import { Card, CardHeader, CardTitle, CardContent } from "@/components/ui/card";
import { Checkbox } from "@/components/ui/checkbox";

interface Todo {
    id: number;
    text: string;
    completed: boolean;
}

const newTodo = ref("");
const todos = ref<Todo[]>([]);

const addTodo = () => {
    if (newTodo.value.trim()) {
        todos.value.push({
            id: Date.now(),
            text: newTodo.value.trim(),
            completed: false,
        });
        newTodo.value = "";
    }
};

const toggleTodo = (id: number) => {
    const todo = todos.value.find((t) => t.id === id);
    if (todo) {
        todo.completed = !todo.completed;
    }
};

const deleteTodo = (id: number) => {
    todos.value = todos.value.filter((t) => t.id !== id);
};
</script>

<template>
<div class="container max-w-2xl mx-auto p-4">
    <Card class="shadow-lg">
    <CardHeader>
        <CardTitle class="text-2xl font-bold text-center">Todo List</CardTitle>
    </CardHeader>
    <CardContent>
        <form @submit.prevent="addTodo" class="flex gap-2 mb-6">
        <Input
            v-model="newTodo"
            placeholder="Add a new todo"
            class="flex-1"
        />
        <Button type="submit">Add</Button>
        </form>
        <div class="space-y-3">
        <div
            v-for="todo in todos"
            :key="todo.id"
            class="flex items-center justify-between p-4 border rounded-lg bg-card text-card-foreground shadow-sm"
        >
            <div class="flex items-center gap-3">
            <Checkbox
                :checked="todo.completed"
                @update:checked="() => toggleTodo(todo.id)"
            />
            <span
                :class="{
                'line-through text-muted-foreground': todo.completed,
                }"
            >
                {{ todo.text }}
            </span>
            </div>
            <Button
            variant="destructive"
            size="sm"
            @click="() => deleteTodo(todo.id)"
            >
            Delete
            </Button>
        </div>
        <div
            v-if="todos.length === 0"
            class="text-center text-muted-foreground py-8"
        >
            No todos yet! Add one above!
        </div>
        </div>
    </CardContent>
    </Card>
</div>
</template>
