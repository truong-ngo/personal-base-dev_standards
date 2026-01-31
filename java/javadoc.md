# Java Documentation Standard
> **Context:** All source code developed within the project (excluding external libraries or generated code).
> **Scope:** All classes, interfaces, enums, and methods under the project namespace.
> **Language:** English (US).

## 1. Class Documentation (File Header)
Every Java file starts with a simple attribution block followed by the Class Javadoc.

### 1.1 The Schema
```text
/*
 * Created by Truong Ngo ([YEAR]).
 */
package [package.name];

import ...;

/**
 * [SHORT_SUMMARY_NOUN_PHRASE].
 * <p>
 * [DETAILED_DESCRIPTION] - Explain the main purpose, key features.
 * Use HTML tags like {@code <ul>} and {@code <li>} for lists.
 * </p>
 *
 * <p><b>Key Features:</b></p>
 * <ul>
 * <li>[Feature 1]</li>
 * <li>[Feature 2]</li>
 * </ul>
 *
 * <p>This class is [Thread-Safety Status] and [intended for inheritance or final].</p>
 *
 * @author Truong Ngo
 * @version [Current_Project_Version]
 * @see [RelatedClass]
 * @since [Since_Version]
 */
public [final] class [ClassName] { ... }
```

### 1.2 Filling Guidelines
* **Header:** Use the simple format: `/* Created by Truong Ngo (2026). */` before the package declaration.
* **Summary:** Start with a Noun Phrase (e.g., `Manager for...`, `Controller handling...`, `Entity representing...`).
* **Metadata:**
    * `@author`: Always `Truong Ngo`.
    * `@version`: Matches the project version.

---

## 2. Public Method Documentation (STRICT)
Every public method **MUST** follow this extended schema. Use the optional sections only when necessary.

### 2.1 The Schema
```text
/**
 * [VERB_3RD_PERSON] [THE_SUBJECT] [CONTEXT/CONDITION].
 * <p>
 * [OPTIONAL: DETAILED EXPLANATION] - Implementation details, thread-safety,
 * or edge case handling.
 * </p>
 *
 * <pre>{@code
 * // [OPTIONAL: EXAMPLE USAGE - Recommended for complex logic]
 * // The '{@code}' tag is REQUIRED inside <pre> to handle Generics correctly
 * List<String> result = ClassUtils.method(args);
 * }</pre>
 *
 * @param <T> [description_of_type] (REQUIRED if method defines a Type Parameter)
 * @param [paramName] the [subject] (may be {@code null} or constraints)
 * @return [RETURN_DESCRIPTION] - Explicitly state behavior for {@code null} results
 * @throws [ExceptionType] if [specific error condition occurs]
 * @see [RelatedClass#method]
 * @since [Version_Number]
 * @deprecated [Reason] (Use {@link #newMethod()} instead)
 */
```

### 2.2 Filling Guidelines
* **Verbs:** Always start with a **3rd person singular** verb (e.g., `Checks`, `Calculates`, `Saves`). **DO NOT** use "Check", "Return", or "This method...".
* **Formatting:** Wrap keywords, class names, and boolean values in code tags: `{@code null}`, `{@code true}`, `{@code String}`.
* **Code Blocks:** Always use the `<pre>{@code ... }</pre>` pattern to prevent HTML escaping issues with Generics.
* **Nullability:**
    * **@param:** Explicitly state `(may be {@code null})` or `(must not be {@code null})`.
    * **@return:** Explicitly state `Returns {@code null} if...` or `(never {@code null})`.
* **Generics:** If the method signature contains `<T>`, you **MUST** include the `@param <T>` tag.

### 2.3 Reference Example (The Gold Standard)
Use this example as the benchmark for quality and style.

```text
/**
 * Instantiates the specified class using its default constructor.
 * <p>
 * This wrapper handles accessibility checks and converts checked reflection
 * exceptions into unchecked {@link IllegalStateException} for cleaner client code.
 * </p>
 *
 * <pre>{@code
 * // Example: Create a new instance of ArrayList
 * List<String> list = ClassUtils.instantiateClass(ArrayList.class);
 * }</pre>
 *
 * @param <T>   the type of the class
 * @param clazz the class to instantiate (must not be {@code null})
 * @return a new instance of the class (never {@code null})
 * @throws IllegalArgumentException if the class is an interface
 * @throws IllegalStateException    if the class has no default constructor
 * @see java.lang.reflect.Constructor#newInstance()
 * @since 1.0.0
 */
public static <T> T instantiateClass(Class<T> clazz) { ... }
```

---

## 3. Private / Internal Method Documentation
Private helper methods follow a relaxed documentation standard. The goal is maintainability, not formal contract.

### 3.1 The Rules
1.  **Format:** Use Javadoc block `/** ... */` (for IDE hover support).
2.  **No Tags:** Do **NOT** use `@param`, `@return` unless necessary for complex algorithms.
3.  **Focus:** Explain **"Why"** and **"Assumptions"** (e.g., "Assumes input is non-null", "Used by method X").

### 3.2 Template & Example
```text
/**
 * Helper for {@link #publicMethod()}.
 * [Short description of specific logic].
 * [Note about assumptions, e.g., "Assumes input is non-null"].
 */
```

**Example:**
```text
/**
 * Helper for {@link #resolveProxy(Object)}.
 * Peels off Hibernate proxies to reveal the real entity class.
 * Assumes the input object is definitely a Hibernate proxy.
 */
private static Class<?> resolveHibernateProxy(Object o) { ... }
```