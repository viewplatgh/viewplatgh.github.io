---
title: how to over-engineer a feature manager - finding the right balance
categories:
  - it-stuff
tags:
  - typescript
  - design-patterns
  - solid
---

# Simplifying Feature Management with Balanced SOLID Principles

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

// Feature manager interfaces
interface IFeatureManagerReader extends ICollectionItemRetriever<IFeature> {}
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

// Example usage
const featureManager = createEnterpriseFeatureManagementSystem()
const darkModeFeature = createFeatureEntity('Dark Mode')
featureManager.performItemAddition(darkModeFeature)
featureManager.enableFeatureByIdentifier(darkModeFeature.getIdentifier())
```

This example showcases several anti-patterns in the name of "following SOLID":

1. **Interface Explosion**: We've created over a dozen interfaces when only a few are needed.
2. **Excessive Abstraction**: Every trivial operation is abstracted behind its own interface.
3. **Verbose Method Naming**: Methods have unnecessarily long names.
4. **Overly Generic Interfaces**: Generic interfaces are created even when they'll only be used for one type.

## A Clean, Simple SOLID Approach with Functional Programming

Now, let's see a truly balanced approach that maintains SOLID principles without overcomplicating things:

```typescript
// Simple feature interface - just what we need
interface Feature {
  name: string
  enabled: boolean
}

// Feature manager with clear, focused functionality
interface FeatureManager {
  add: (feature: Feature) => void
  get: (name: string) => Feature | undefined
  getAll: () => Feature[]
  enable: (name: string) => void
  disable: (name: string) => void
  toggle: (name: string) => void
  isEnabled: (name: string) => boolean
}

// Create a new feature
function createFeature(name: string): Feature {
  return {
    name,
    enabled: false,
  }
}

// Create a feature manager
function createFeatureManager(): FeatureManager {
  const features: Record<string, Feature> = {}

  return {
    add(feature: Feature) {
      features[feature.name] = feature
    },

    get(name: string) {
      return features[name]
    },

    getAll() {
      return Object.values(features)
    },

    enable(name: string) {
      const feature = features[name]
      if (feature) {
        feature.enabled = true
      }
    },

    disable(name: string) {
      const feature = features[name]
      if (feature) {
        feature.enabled = false
      }
    },

    toggle(name: string) {
      const feature = features[name]
      if (feature) {
        feature.enabled = !feature.enabled
      }
    },

    isEnabled(name: string) {
      const feature = features[name]
      return feature ? feature.enabled : false
    },
  }
}

// Example usage
const featureManager = createFeatureManager()
featureManager.add(createFeature('darkMode'))
featureManager.enable('darkMode')
console.log(featureManager.isEnabled('darkMode')) // true
```

This implementation still follows SOLID principles, but in a much more pragmatic way:

1. **Single Responsibility Principle**: The feature manager handles feature operations while features themselves are just data.

2. **Open/Closed Principle**: You can extend functionality by creating wrapper functions around the feature manager without modifying its code.

3. **Liskov Substitution Principle**: Any implementation of the FeatureManager interface can be substituted as long as it follows the contract.

4. **Interface Segregation Principle**: The interface is focused and minimal, providing only what clients need.

5. **Dependency Inversion Principle**: Code that uses the feature manager depends on its interface, not its implementation.

## The SOLID Middle Ground

This simplified example demonstrates that SOLID principles don't require complex abstractions or dozens of interfaces. The key insights:

1. **Simplicity Matters**: The simplest solution that meets the requirements is often the best.

2. **Practical Interfaces**: Design interfaces around actual use cases, not theoretical abstractions.

3. **Minimize Indirection**: Each layer of abstraction should provide clear value.

4. **Focus on Intent**: Code should clearly express what it's doing without obscuring it behind excessive abstractions.

## Conclusion

SOLID principles provide valuable guidance for software design, but they must be applied with common sense and pragmatism. The over-engineered example demonstrates how misapplying these principles leads to unnecessarily complex code, while the simplified approach shows that good design can also be straightforward.

When applying SOLID principles:

- Start simple and add complexity only when needed
- Design interfaces based on actual client needs
- Use the simplest solution that satisfies the requirements
- Value readability and maintainability above perfect abstraction

Remember that the ultimate goal is to write code that solves problems effectively and is easy to understand, extend, and maintain. Sometimes, the most elegant solution is also the simplest one.
