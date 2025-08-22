Great question 👍 — the **manifest** is one of those “hidden but important” things in a JAR file.

---

## 1. What is a JAR?

* A **JAR file** (`.jar`) is basically a **ZIP archive** with:

  * Your compiled `.class` files
  * Resources (images, config files, etc.)
  * **Metadata**

That metadata lives in the **manifest file**.

---

## 2. What is the manifest?

* The **manifest** is a special file located at:

  ```
  META-INF/MANIFEST.MF
  ```
* It’s a simple text file with **key–value pairs** that describe the JAR’s contents and properties.

Example:

```
Manifest-Version: 1.0
Created-By: Gradle 9.0
Main-Class: com.example.Main
```

---

## 3. What is it used for?

The manifest can contain many attributes. The most important ones are:

* **`Manifest-Version`** → version of the manifest file format (usually `1.0`).
* **`Main-Class`** → the entry point class (with `public static void main` method).

  * This makes the JAR *executable*.
  * Example:

    ```bash
    java -jar myapp.jar
    ```

    Java looks into the manifest, finds `Main-Class`, and launches it.
* **`Class-Path`** → tells the JVM what other JARs are needed.

  ```
  Class-Path: lib/other.jar lib/more.jar
  ```
* **Custom metadata** → some frameworks and tools add extra info, like version, vendor, or build tool.

---

## 4. Manifest in Gradle

In Gradle, when you use the `jar` task, you can configure the manifest:

```gradle
tasks.named("jar") {
    manifest {
        attributes(
            "Implementation-Title" to "My App",
            "Implementation-Version" to "1.0",
            "Main-Class" to "com.example.Main"
        )
    }
}
```

This will produce a `META-INF/MANIFEST.MF` inside your JAR with those entries.

---

## 5. How to see the manifest of a JAR

You can open it like a ZIP file:

```bash
jar xf myapp.jar META-INF/MANIFEST.MF
cat META-INF/MANIFEST.MF
```

or with any archive viewer.

---

✅ **In short:**
The manifest is the **metadata file inside a JAR**. It tells tools (like the JVM) how to use the JAR (main class, dependencies, version, etc.), and can also carry custom info added by build tools or frameworks.

---

Would you like me to show you a **real example manifest from a Gradle-built JAR** (what Gradle puts by default vs. when you configure it)?
