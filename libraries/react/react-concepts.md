## Grouping / Ordering properties of Component.

1. Context and Router hooks (external dependencies)
2. Memoized values (computed once)  
3. Component state (local state management)
4. Memoized callbacks (functions that depend on state)
5. Custom hooks (composed logic)
6. Derived state (computed from other state)
7. Side effects (useEffect hooks)

### Benefits:
✅ Dependency Order: Dependencies come before dependents
✅ Related Items Grouped: All state, all callbacks, all effects together
✅ Easier to Follow: Clear logical flow from top to bottom
✅ Better Maintainability: Easy to find and modify related code
✅ React Best Practices: Follows common React component structure patterns


## Key React Principle
- useState: React internally tracks state and only re-initializes on mount
- useContext: React efficiently returns the current context value
- useHistory: React Router returns a stable history object
- useMemo: for the tomorrow value if you wanted to optimize the date calculation:
- useCallback: for memoization to prevent unnecessary re-renders and optimize performance.

### useState
- Each useState call maintains its own persistent state across re-renders, even inside custom hooks.
- React internally maintains a "hook state array" for each component instance:
- React internally maintains a "state hook index" for each component instance:
- The initial value in useState<T>() is only used on the very first render. On all subsequent re-renders, React preserves the current state value.

State only gets reset to the initial value when:
- Component unmounts and remounts (completely new component instance)
- Key prop changes (forces React to create a new component instance)
- Explicitly calling setMixes([]) (manual reset)

The hook can have a guard mechanism that prevents the expensive operations from running multiple times:
e.g. 
```
function useOnetimeCustomHook() {
  const [foo, setFoo] = useState(false);

  useEffect(() => {
    if (foo) return
    
    :
    setFoo(true);
  })

  return { foo };
}

export default MainComponent() {
  :
  const { foo } = useOnetimeCustomHook();
  :
}
```
- On First Render:
  useState(false) creates new state with value false
- isInitialized = false
  On Subsequent Re-renders:
- useState(false) ignores the initial value false
  Returns the current state value (which becomes true after initialization)
- isInitialized = true (preserved from previous render)


### useMemo
If you wanted to avoid even the hook call overhead, you could move the hook results to a useMemo

### useCallback
Benefits
- Function Reference Stability: Without useCallback, this function would be recreated on every render of the ProductionView component. This means a new function reference would be created each time.
- Performance Optimization: if the function is used in multiple places in the component
- Child Component Props: If this function were passed down to child components as a prop, those child components would re-render unnecessarily every time the parent renders (because they'd receive a "new" function reference each time).
- Array Methods: If the function is used in .filter() operations on an array object. Without memoization, these filter operations might be less efficient.
