---
title: how to over-engineer a feature manager - finding the right balance
categories:
  - it-stuff
tags:
  - typescript
  - design-patterns
  - solid
---

When applying design principles like SOLID, balance is key. It's easy to fall into the trap of over-engineering by applying principles too rigidly or extensively. Today, I want to explore a real-world example: implementing a feature flag manager in TypeScript using functional programming approaches.

## The Over-Engineered Example: A Cautionary Tale

Let's start by looking at an implementation that takes SOLID principles to an absurd extreme with functional programming:

```typescript
// A galaxy of ultra-granular interfaces
interface INameable {
  getName(): string
}

interface IRenameable {
  setName(name: string): void
}

interface IEnablementStatusProvider {
  getEnablementStatus(): boolean
}

interface IEnablementStatusModifier {
  setEnablementStatus(status: boolean): void
}

interface ITogglable {
  performToggleOperation(): void
}

interface IIdentifiable {
  getIdentifier(): string
}

// Composite interfaces
interface IFeatureMetadata extends INameable, IIdentifiable {}
interface IFeatureStateReader extends IEnablementStatusProvider {}
interface IFeatureStateWriter extends IEnablementStatusModifier, ITogglable {}

// Main interfaces
interface IFeature
  extends IFeatureMetadata,
    IFeatureStateReader,
    IFeatureStateWriter,
    IRenameable {}

// Collection interfaces
interface ICollectionItemAdder<T extends IIdentifiable> {
  performItemAddition(item: T): boolean
}

interface ICollectionItemRemover<T extends IIdentifiable> {
  performItemRemoval(identifier: string): boolean
}

interface ICollectionItemRetriever<T extends IIdentifiable> {
  retrieveItemByIdentifier(identifier: string): T | null
}

interface ICollectionItemsLister<T extends IIdentifiable> {
  retrieveAllItems(): ReadonlyArray<T>
}

interface ICollectionItemCounter {
  getItemCount(): number
}

// Feature manager interfaces
interface IFeatureManagerReader
  extends ICollectionItemRetriever<IFeature>,
    ICollectionItemsLister<IFeature>,
    ICollectionItemCounter {}
interface IFeatureManagerWriter
  extends ICollectionItemAdder<IFeature>,
    ICollectionItemRemover<IFeature> {}
interface IFeatureManagerToggler {
  toggleFeatureByIdentifier(identifier: string): boolean
  enableFeatureByIdentifier(identifier: string): boolean
  disableFeatureByIdentifier(identifier: string): boolean
}

interface IFeatureManagerComplete
  extends IFeatureManagerReader,
    IFeatureManagerWriter,
    IFeatureManagerToggler {}

// Feature factory
let idCounter = 0

function createFeatureEntity(name: string): IFeature {
  const featureId = `feature_${++idCounter}`
  let featureName = name
  let enablementStatus = false

  return {
    getName(): string {
      return featureName
    },

    setName(name: string): void {
      featureName = name
    },

    getIdentifier(): string {
      return featureId
    },

    getEnablementStatus(): boolean {
      return enablementStatus
    },

    setEnablementStatus(status: boolean): void {
      enablementStatus = status
    },

    performToggleOperation(): void {
      enablementStatus = !enablementStatus
    },
  }
}

// Feature manager implementation
function createEnterpriseFeatureManagementSystem(): IFeatureManagerComplete {
  const featureEntities: Map<string, IFeature> = new Map<string, IFeature>()

  return {
    performItemAddition(item: IFeature): boolean {
      if (featureEntities.has(item.getIdentifier())) {
        return false
      }
      featureEntities.set(item.getIdentifier(), item)
      return true
    },

    performItemRemoval(identifier: string): boolean {
      return featureEntities.delete(identifier)
    },

    retrieveItemByIdentifier(identifier: string): IFeature | null {
      return featureEntities.get(identifier) || null
    },

    retrieveAllItems(): ReadonlyArray<IFeature> {
      return Array.from(featureEntities.values())
    },

    getItemCount(): number {
      return featureEntities.size
    },

    toggleFeatureByIdentifier(identifier: string): boolean {
      const feature = featureEntities.get(identifier) || null
      if (feature === null) {
        return false
      }
      feature.performToggleOperation()
      return true
    },

    enableFeatureByIdentifier(identifier: string): boolean {
      const feature = featureEntities.get(identifier) || null
      if (feature === null) {
        return false
      }
      feature.setEnablementStatus(true)
      return true
    },

    disableFeatureByIdentifier(identifier: string): boolean {
      const feature = featureEntities.get(identifier) || null
      if (feature === null) {
        return false
      }
      feature.setEnablementStatus(false)
      return true
    },
  }
}

// Example usage
const featureManager = createEnterpriseFeatureManagementSystem()
const darkModeFeature = createFeatureEntity('Dark Mode')
featureManager.performItemAddition(darkModeFeature)
featureManager.enableFeatureByIdentifier(darkModeFeature.getIdentifier())
```

This example showcases several anti-patterns in the name of "following SOLID":

1. **Interface Explosion**: We've created 14 separate interfaces when only a handful are actually needed.
2. **Excessive Abstraction**: Every trivial operation is abstracted behind its own interface.
3. **Verbose Method Naming**: Methods are unnecessarily verbose (e.g., `performItemAddition` instead of `add`).
4. **Overly Generic Interfaces**: Generic interfaces are created even when they'll only be used for one type.
5. **Enterprise Syndrome**: Functions and interfaces have long, grandiose names that add complexity without value.
6. **Poor Functional Design**: The functional implementation still tries to mimic OOP patterns rather than leveraging functional programming's strengths.

## A Clean, Professional SOLID Approach with Functional Programming

Now, let's see how we can apply SOLID principles correctly with functional programming to build a maintainable, extensible feature management system:

```typescript
// 1. Single Responsibility Principle: Each interface has one clear responsibility
interface Feature {
  readonly id: string
  readonly name: string
  enabled: boolean
}

// 2. Interface Segregation Principle: Separated interfaces for different concerns
interface FeatureProvider {
  getFeature: (id: string) => Feature | undefined
  getFeatureByName: (name: string) => Feature | undefined
  getAllFeatures: () => Feature[]
}

interface FeatureRegistry {
  addFeature: (feature: Feature) => void
  removeFeature: (id: string) => void
}

interface FeatureToggler {
  enableFeature: (id: string) => boolean
  disableFeature: (id: string) => boolean
  toggleFeature: (id: string) => boolean
}

interface FeatureManager
  extends FeatureProvider,
    FeatureRegistry,
    FeatureToggler {}

// 3. Pure functions for feature operations (Liskov Substitution)
function createFeature(id: string, name: string): Feature {
  return {
    id,
    name,
    enabled: false,
  }
}

function enableFeature(feature: Feature): Feature {
  return { ...feature, enabled: true }
}

function disableFeature(feature: Feature): Feature {
  return { ...feature, enabled: false }
}

function toggleFeature(feature: Feature): Feature {
  return { ...feature, enabled: !feature.enabled }
}

// 4. Factory function for feature manager (Open/Closed Principle)
function createFeatureManager(): FeatureManager {
  // Private state
  const features: Map<string, Feature> = new Map()
  const nameIndex: Map<string, string> = new Map()

  // Helper functions
  function updateFeature(
    id: string,
    updater: (feature: Feature) => Feature
  ): boolean {
    const feature = features.get(id)
    if (!feature) return false

    const updatedFeature = updater(feature)
    features.set(id, updatedFeature)
    return true
  }

  // Public interface
  return {
    // FeatureProvider implementation
    getFeature(id: string): Feature | undefined {
      return features.get(id)
    },

    getFeatureByName(name: string): Feature | undefined {
      const id = nameIndex.get(name)
      if (!id) return undefined
      return features.get(id)
    },

    getAllFeatures(): Feature[] {
      return Array.from(features.values())
    },

    // FeatureRegistry implementation
    addFeature(feature: Feature): void {
      features.set(feature.id, feature)
      nameIndex.set(feature.name, feature.id)
    },

    removeFeature(id: string): void {
      const feature = features.get(id)
      if (feature) {
        features.delete(id)
        nameIndex.delete(feature.name)
      }
    },

    // FeatureToggler implementation
    enableFeature(id: string): boolean {
      return updateFeature(id, enableFeature)
    },

    disableFeature(id: string): boolean {
      return updateFeature(id, disableFeature)
    },

    toggleFeature(id: string): boolean {
      return updateFeature(id, toggleFeature)
    },
  }
}

// 5. Dependency Inversion Principle: Higher-order functions depend on abstractions
function createFeatureClient(featureProvider: FeatureProvider) {
  return {
    isFeatureEnabled(name: string): boolean {
      const feature = featureProvider.getFeatureByName(name)
      return feature ? feature.enabled : false
    },

    withFeature<T>(
      name: string,
      handler: (feature: Feature) => T,
      fallback: T
    ): T {
      const feature = featureProvider.getFeatureByName(name)
      return feature ? handler(feature) : fallback
    },
  }
}

// Example usage
const featureManager = createFeatureManager()
const feature = createFeature('f1', 'darkMode')
featureManager.addFeature(feature)
featureManager.enableFeature('f1')

const client = createFeatureClient(featureManager)
console.log(client.isFeatureEnabled('darkMode')) // true
```

This implementation applies SOLID principles in a balanced way using functional programming:

1. **Single Responsibility Principle**: Feature data structure is separate from operations on features, which are separate from the feature collection management.

2. **Open/Closed Principle**: The system is extensible. You can add new feature operations or specialized feature managers without modifying existing code.

3. **Liskov Substitution Principle**: Pure functions operate on the `Feature` interface, making it easy to substitute different implementations of features.

4. **Interface Segregation Principle**: `FeatureProvider`, `FeatureRegistry`, and `FeatureToggler` allow clients to depend only on the functionality they need.

5. **Dependency Inversion Principle**: Higher-order functions like `createFeatureClient` depend on abstractions like `FeatureProvider`, not concrete implementations.

## Finding the Right Balance

The balanced functional implementation offers several advantages over the over-engineered one:

1. **Immutability**: Pure functions like `toggleFeature` create new feature objects rather than mutating state.
2. **Composability**: Functions can be easily composed to create more complex operations.
3. **Simplicity**: The interface is clean and intuitive, focusing on what matters.
4. **Testability**: Pure functions are extremely easy to test since they have no side effects.
5. **Separation of Concerns**: Data structures are separate from operations on them.

## Conclusion

SOLID principles provide valuable guidance for software design, but they must be applied with judgment and balance, regardless of whether you're using object-oriented or functional programming paradigms.

The over-engineered example demonstrates how misapplying these principles leads to unnecessarily complex code that's hard to understand and maintain. The balanced approach shows how we can adhere to good design principles while keeping our code clean and understandable.

When using functional programming, SOLID principles can still be applied, though the implementation details differ from object-oriented approaches:

- Functions replace classes for implementing behavior
- Data structures are often immutable
- Higher-order functions provide composition and abstraction
- Pure functions promote testability and reasoning

Remember that design principles are tools to help us create better software, not rigid rules to follow at all costs. The ultimate goal is to write code that's easy to understand, extend, and maintain.
