# Antigravity Common Patterns

**Reusable patterns and best practices for Antigravity projects.**

## API Response Format

### Standard Response Structure
```typescript
interface ApiResponse<T> {
  success: boolean
  data?: T
  error?: string
  meta?: {
    total?: number
    page?: number
    limit?: number
    timestamp?: string
  }
}
```

### Success Response
```typescript
return {
  success: true,
  data: {
    id: '123',
    name: 'John Doe'
  },
  meta: {
    timestamp: new Date().toISOString()
  }
}
```

### Error Response
```typescript
return {
  success: false,
  error: 'User not found',
  meta: {
    timestamp: new Date().toISOString()
  }
}
```

### Paginated Response
```typescript
return {
  success: true,
  data: items,
  meta: {
    total: 100,
    page: 1,
    limit: 20,
    timestamp: new Date().toISOString()
  }
}
```

## Custom Hooks Pattern

### Debounce Hook
```typescript
export function useDebounce<T>(value: T, delay: number): T {
  const [debouncedValue, setDebouncedValue] = useState<T>(value)

  useEffect(() => {
    const handler = setTimeout(() => {
      setDebouncedValue(value)
    }, delay)

    return () => {
      clearTimeout(handler)
    }
  }, [value, delay])

  return debouncedValue
}
```

### Local Storage Hook
```typescript
export function useLocalStorage<T>(key: string, initialValue: T) {
  const [storedValue, setStoredValue] = useState<T>(() => {
    try {
      const item = window.localStorage.getItem(key)
      return item ? JSON.parse(item) : initialValue
    } catch (error) {
      return initialValue
    }
  })

  const setValue = (value: T | ((val: T) => T)) => {
    try {
      const valueToStore = value instanceof Function ? value(storedValue) : value
      setStoredValue(valueToStore)
      window.localStorage.setItem(key, JSON.stringify(valueToStore))
    } catch (error) {
      console.error('Error saving to localStorage:', error)
    }
  }

  return [storedValue, setValue] as const
}
```

### Async Data Hook
```typescript
export function useAsync<T>(asyncFunction: () => Promise<T>) {
  const [data, setData] = useState<T | null>(null)
  const [loading, setLoading] = useState(true)
  const [error, setError] = useState<Error | null>(null)

  useEffect(() => {
    let isMounted = true

    asyncFunction()
      .then(result => {
        if (isMounted) {
          setData(result)
          setLoading(false)
        }
      })
      .catch(err => {
        if (isMounted) {
          setError(err)
          setLoading(false)
        }
      })

    return () => {
      isMounted = false
    }
  }, [])

  return { data, loading, error }
}
```

## Repository Pattern

### Interface Definition
```typescript
interface Repository<T> {
  findAll(filters?: Filters): Promise<T[]>
  findById(id: string): Promise<T | null>
  create(data: CreateDto): Promise<T>
  update(id: string, data: UpdateDto): Promise<T>
  delete(id: string): Promise<void>
}
```

### Implementation Example
```typescript
export class UserRepository implements Repository<User> {
  async findAll(filters?: UserFilters): Promise<User[]> {
    let query = supabase.from('users').select('*')
    
    if (filters?.isActive) {
      query = query.eq('is_active', true)
    }
    
    const { data, error } = await query
    if (error) throw error
    return data
  }

  async findById(id: string): Promise<User | null> {
    const { data, error } = await supabase
      .from('users')
      .select('*')
      .eq('id', id)
      .single()
    
    if (error) return null
    return data
  }

  async create(data: CreateUserDto): Promise<User> {
    const { data: user, error } = await supabase
      .from('users')
      .insert(data)
      .select()
      .single()
    
    if (error) throw error
    return user
  }

  async update(id: string, data: UpdateUserDto): Promise<User> {
    const { data: user, error } = await supabase
      .from('users')
      .update(data)
      .eq('id', id)
      .select()
      .single()
    
    if (error) throw error
    return user
  }

  async delete(id: string): Promise<void> {
    const { error } = await supabase
      .from('users')
      .delete()
      .eq('id', id)
    
    if (error) throw error
  }
}
```

## Service Layer Pattern

### Service Interface
```typescript
export class UserService {
  constructor(private userRepository: UserRepository) {}

  async getActiveUsers(): Promise<User[]> {
    return this.userRepository.findAll({ isActive: true })
  }

  async getUserById(id: string): Promise<User> {
    const user = await this.userRepository.findById(id)
    if (!user) {
      throw new Error(`User not found: ${id}`)
    }
    return user
  }

  async createUser(data: CreateUserDto): Promise<User> {
    // Business logic here
    this.validateUserData(data)
    return this.userRepository.create(data)
  }

  private validateUserData(data: CreateUserDto): void {
    if (!data.email.includes('@')) {
      throw new Error('Invalid email format')
    }
  }
}
```

## Error Handling Pattern

### Custom Error Classes
```typescript
export class AppError extends Error {
  constructor(
    public message: string,
    public statusCode: number = 500,
    public isOperational: boolean = true
  ) {
    super(message)
    Object.setPrototypeOf(this, AppError.prototype)
  }
}

export class NotFoundError extends AppError {
  constructor(message: string) {
    super(message, 404)
  }
}

export class ValidationError extends AppError {
  constructor(message: string) {
    super(message, 400)
  }
}

export class UnauthorizedError extends AppError {
  constructor(message: string = 'Unauthorized') {
    super(message, 401)
  }
}
```

### Error Handler Middleware
```typescript
export function errorHandler(
  error: Error,
  req: Request,
  res: Response,
  next: NextFunction
) {
  if (error instanceof AppError) {
    return res.status(error.statusCode).json({
      success: false,
      error: error.message
    })
  }

  // Log unexpected errors
  console.error('Unexpected error:', error)

  return res.status(500).json({
    success: false,
    error: 'Internal server error'
  })
}
```

## Validation Pattern

### Zod Schema Validation
```typescript
import { z } from 'zod'

export const CreateUserSchema = z.object({
  email: z.string().email(),
  name: z.string().min(2).max(100),
  age: z.number().int().min(18).max(120).optional()
})

export type CreateUserDto = z.infer<typeof CreateUserSchema>

// Usage in API route
export async function POST(req: Request) {
  try {
    const body = await req.json()
    const validatedData = CreateUserSchema.parse(body)
    
    // Use validatedData (type-safe)
    const user = await createUser(validatedData)
    
    return Response.json({ success: true, data: user })
  } catch (error) {
    if (error instanceof z.ZodError) {
      return Response.json(
        { success: false, error: error.errors },
        { status: 400 }
      )
    }
    throw error
  }
}
```

## Caching Pattern

### Simple In-Memory Cache
```typescript
export class Cache<T> {
  private cache = new Map<string, { value: T; expiry: number }>()

  set(key: string, value: T, ttlMs: number): void {
    const expiry = Date.now() + ttlMs
    this.cache.set(key, { value, expiry })
  }

  get(key: string): T | null {
    const item = this.cache.get(key)
    if (!item) return null

    if (Date.now() > item.expiry) {
      this.cache.delete(key)
      return null
    }

    return item.value
  }

  delete(key: string): void {
    this.cache.delete(key)
  }

  clear(): void {
    this.cache.clear()
  }
}
```

### Redis Cache Pattern
```typescript
export class RedisCache {
  constructor(private redis: Redis) {}

  async get<T>(key: string): Promise<T | null> {
    const value = await this.redis.get(key)
    return value ? JSON.parse(value) : null
  }

  async set<T>(key: string, value: T, ttlSeconds: number): Promise<void> {
    await this.redis.setex(key, ttlSeconds, JSON.stringify(value))
  }

  async delete(key: string): Promise<void> {
    await this.redis.del(key)
  }
}
```

## Retry Pattern

### Exponential Backoff Retry
```typescript
export async function retryWithBackoff<T>(
  fn: () => Promise<T>,
  maxRetries: number = 3,
  baseDelayMs: number = 1000
): Promise<T> {
  let lastError: Error

  for (let attempt = 0; attempt < maxRetries; attempt++) {
    try {
      return await fn()
    } catch (error) {
      lastError = error as Error
      
      if (attempt < maxRetries - 1) {
        const delay = baseDelayMs * Math.pow(2, attempt)
        await new Promise(resolve => setTimeout(resolve, delay))
      }
    }
  }

  throw lastError!
}
```

## Skeleton Projects Pattern

When implementing new functionality:

### 1. Search for Battle-Tested Skeletons
```
User: I need to implement OAuth authentication

Antigravity: Let me search for proven OAuth implementations...
[Searches GitHub, npm, documentation]
```

### 2. Evaluate Options with Parallel Agents
```
Use multiple agents to assess:
- Security assessment (security-reviewer agent)
- Extensibility analysis (architect agent)
- Relevance scoring (planner agent)
- Implementation planning (planner agent)
```

### 3. Clone Best Match as Foundation
```bash
# Clone the skeleton project
git clone https://github.com/example/oauth-skeleton.git

# Extract relevant parts
cp -r oauth-skeleton/src/auth ./src/
```

### 4. Iterate Within Proven Structure
```
- Keep the proven architecture
- Customize for your needs
- Add tests
- Document changes
```

## Immutability Pattern

### Spread Operators
```typescript
// ❌ Bad - Mutation
user.name = 'John'
array.push(item)
object.property = value

// ✅ Good - Immutable
const updatedUser = { ...user, name: 'John' }
const updatedArray = [...array, item]
const updatedObject = { ...object, property: value }
```

### Array Operations
```typescript
// Add item
const newArray = [...oldArray, newItem]

// Remove item
const filtered = oldArray.filter(item => item.id !== removeId)

// Update item
const updated = oldArray.map(item =>
  item.id === updateId ? { ...item, name: 'New Name' } : item
)

// Sort (without mutating)
const sorted = [...oldArray].sort((a, b) => a.value - b.value)
```

## Best Practices

### DO
- ✅ Use these patterns consistently
- ✅ Adapt patterns to your needs
- ✅ Document deviations from patterns
- ✅ Share new patterns with team
- ✅ Keep patterns simple and focused

### DON'T
- ❌ Blindly follow patterns without understanding
- ❌ Over-engineer simple solutions
- ❌ Mix multiple patterns inconsistently
- ❌ Ignore project-specific requirements
- ❌ Create patterns for one-off use cases

**Remember**: Patterns are guidelines, not rules. Use them to solve problems, not to create complexity.
