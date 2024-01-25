# RSC (React Server Component)

### 1. React Query vs RSC
- Most of the React Query use cases could be replaced with RSC
    - Queries: with Initial data or Hydrate
    - Mutation: with server action
- Some features of React Query are not replacable with RSC
    - Infinite scrolling items
    - Interval refetch (showing fresh data even without user interactions)
    - Offline queries that do not require network
    - Environments outside Web (eg. React Native)