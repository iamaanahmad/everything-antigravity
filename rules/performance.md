# Antigravity Performance Optimization

**Performance guidelines for Antigravity projects and AI-assisted development.**

## Model Selection Strategy

Choose the right Gemini model for the task:

### Gemini 2.0 Flash (Fast & Efficient)
**Use for:**
- Lightweight agents with frequent invocation
- Pair programming and code generation
- Worker agents in multi-agent systems
- Simple refactoring tasks
- Documentation updates
- Code reviews (non-security)

**Characteristics:**
- Fast response time
- Lower cost
- Good for 90% of tasks
- Suitable for iterative workflows

### Gemini 2.0 Pro Exp (Balanced)
**Use for:**
- Main development work
- Orchestrating multi-agent workflows
- Complex coding tasks
- Architectural decisions
- Security reviews
- Test generation

**Characteristics:**
- Best coding model
- Balanced speed and capability
- Default choice for most agents
- Good reasoning ability

### Gemini 2.0 Ultra (Deep Reasoning)
**Use for:**
- Complex architectural decisions
- Maximum reasoning requirements
- Research and analysis tasks
- Novel problem solving
- System design
- Critical security analysis

**Characteristics:**
- Deepest reasoning
- Slower response time
- Higher cost
- Use sparingly for complex tasks

## Context Window Management

### Avoid Last 20% of Context Window

**High context sensitivity tasks:**
- Large-scale refactoring
- Feature implementation spanning multiple files
- Debugging complex interactions
- Multi-file code generation

**Strategy:**
- Monitor context usage
- Break large tasks into smaller chunks
- Use subagents for isolated work
- Clear context when approaching limit

### Lower Context Sensitivity Tasks

**Safe in high context usage:**
- Single-file edits
- Independent utility creation
- Documentation updates
- Simple bug fixes
- Code formatting

## Enhanced Thinking Mode

For complex tasks requiring deep reasoning:

### 1. Use Enhanced Thinking
```
User: [Enable enhanced thinking mode]
I need to design a distributed caching system
```

### 2. Enable Plan Mode
```
User: [Plan mode enabled]
Break down the implementation into phases
```

### 3. "Rev the Engine"
```
Multiple critique rounds:
1. Initial design
2. Security review
3. Performance analysis
4. Scalability assessment
5. Final refinement
```

### 4. Split Role Sub-Agents
```
Use diverse agents for analysis:
- Architect agent: System design
- Security reviewer: Vulnerability analysis
- Performance optimizer: Bottleneck identification
- Planner: Implementation strategy
```

## Build Troubleshooting

### Incremental Error Fixing

**Use build-error-resolver agent:**

```
User: Build is failing with TypeScript errors

Antigravity: [Invokes build-error-resolver agent]

Build Error Resolver:
1. Collecting all errors...
2. Categorizing by severity...
3. Fixing errors one by one...
4. Verifying after each fix...
```

**Strategy:**
1. Fix one error at a time
2. Verify build after each fix
3. Don't introduce new errors
4. Minimal diffs only

## Code Performance

### Algorithm Complexity

**Be mindful of Big O:**
```typescript
// ❌ Bad - O(n²)
for (const item1 of array1) {
  for (const item2 of array2) {
    if (item1.id === item2.id) {
      // Do something
    }
  }
}

// ✅ Good - O(n)
const map = new Map(array2.map(item => [item.id, item]))
for (const item1 of array1) {
  const item2 = map.get(item1.id)
  if (item2) {
    // Do something
  }
}
```

### Use Appropriate Data Structures

```typescript
// For lookups: Use Map or Set
const userMap = new Map(users.map(u => [u.id, u]))
const activeIds = new Set(activeUsers.map(u => u.id))

// For ordered data: Use Array
const sortedItems = items.sort((a, b) => a.priority - b.priority)

// For unique values: Use Set
const uniqueValues = new Set(values)
```

### Memoization

```typescript
// Expensive calculation
const memoizedCalculation = useMemo(() => {
  return expensiveOperation(data)
}, [data])

// Expensive component
const MemoizedComponent = memo(({ data }) => {
  return <ExpensiveRender data={data} />
})
```

## Database Performance

### Query Optimization

```typescript
// ❌ Bad - N+1 queries
const users = await getUsers()
for (const user of users) {
  user.posts = await getPosts(user.id) // N queries
}

// ✅ Good - Single query with join
const users = await supabase
  .from('users')
  .select('*, posts(*)')
```

### Indexing

```sql
-- Add indexes for frequently queried columns
CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_posts_user_id ON posts(user_id);
CREATE INDEX idx_posts_created_at ON posts(created_at DESC);
```

### Pagination

```typescript
// Always paginate large datasets
const { data, error } = await supabase
  .from('posts')
  .select('*')
  .range(0, 19) // First 20 items
  .order('created_at', { ascending: false })
```

## Frontend Performance

### Code Splitting

```typescript
// Lazy load routes
const Dashboard = lazy(() => import('./pages/Dashboard'))
const Settings = lazy(() => import('./pages/Settings'))

// Lazy load heavy components
const Chart = lazy(() => import('./components/Chart'))
```

### Image Optimization

```typescript
// Use Next.js Image component
import Image from 'next/image'

<Image
  src="/hero.jpg"
  alt="Hero"
  width={1200}
  height={600}
  priority // For above-the-fold images
/>
```

### Debouncing

```typescript
// Debounce search input
const debouncedSearch = useDebounce(searchQuery, 300)

useEffect(() => {
  if (debouncedSearch) {
    performSearch(debouncedSearch)
  }
}, [debouncedSearch])
```

## Bundle Size Optimization

### Tree Shaking

```typescript
// ❌ Bad - Imports entire library
import _ from 'lodash'

// ✅ Good - Import only what you need
import debounce from 'lodash/debounce'
import throttle from 'lodash/throttle'
```

### Dynamic Imports

```typescript
// Load heavy library only when needed
async function processData(data) {
  const { default: heavyLib } = await import('heavy-library')
  return heavyLib.process(data)
}
```

### Analyze Bundle

```bash
# Analyze bundle size
npm run build -- --analyze

# Check what's in the bundle
npx webpack-bundle-analyzer dist/stats.json
```

## Caching Strategies

### API Response Caching

```typescript
// Cache API responses
const cache = new Map()

async function fetchWithCache(url: string, ttl: number = 60000) {
  const cached = cache.get(url)
  if (cached && Date.now() - cached.timestamp < ttl) {
    return cached.data
  }

  const data = await fetch(url).then(r => r.json())
  cache.set(url, { data, timestamp: Date.now() })
  return data
}
```

### Redis Caching

```typescript
// Cache expensive operations in Redis
async function getMarkets() {
  const cached = await redis.get('markets')
  if (cached) return JSON.parse(cached)

  const markets = await fetchMarketsFromDB()
  await redis.setex('markets', 300, JSON.stringify(markets)) // 5 min TTL
  return markets
}
```

## Monitoring and Profiling

### Performance Monitoring

```typescript
// Measure operation time
console.time('operation')
await expensiveOperation()
console.timeEnd('operation')

// Use Performance API
const start = performance.now()
await operation()
const duration = performance.now() - start
console.log(`Operation took ${duration}ms`)
```

### React Profiler

```typescript
import { Profiler } from 'react'

<Profiler id="Dashboard" onRender={onRenderCallback}>
  <Dashboard />
</Profiler>

function onRenderCallback(
  id, phase, actualDuration, baseDuration, startTime, commitTime
) {
  console.log(`${id} took ${actualDuration}ms`)
}
```

## Agent Performance

### Parallel Agent Execution

```
When possible, run agents in parallel:

User: Review security and performance

Antigravity:
[Invokes security-reviewer agent] (parallel)
[Invokes performance analyzer] (parallel)

Results combined after both complete
```

### Agent Context Optimization

```
Keep agent contexts focused:
- Provide only relevant files
- Use specific file paths
- Avoid loading entire codebase
- Clear context between tasks
```

## Best Practices

### DO
- ✅ Choose appropriate model for task
- ✅ Monitor context window usage
- ✅ Use memoization for expensive operations
- ✅ Optimize database queries
- ✅ Implement caching strategies
- ✅ Lazy load heavy components
- ✅ Profile before optimizing
- ✅ Measure performance improvements

### DON'T
- ❌ Use Ultra model for simple tasks
- ❌ Ignore context window limits
- ❌ Premature optimization
- ❌ Load entire datasets without pagination
- ❌ Skip caching for expensive operations
- ❌ Import entire libraries
- ❌ Optimize without measuring
- ❌ Sacrifice readability for micro-optimizations

## Performance Checklist

Before deploying:
- [ ] Database queries optimized
- [ ] Indexes added for frequent queries
- [ ] API responses cached
- [ ] Images optimized
- [ ] Code split by route
- [ ] Heavy libraries lazy loaded
- [ ] Bundle size analyzed
- [ ] Performance metrics measured
- [ ] No N+1 queries
- [ ] Pagination implemented

**Remember**: Premature optimization is the root of all evil. Measure first, optimize second. Use the right tool (model) for the job.
