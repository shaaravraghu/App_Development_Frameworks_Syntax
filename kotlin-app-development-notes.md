# 🚀 Comprehensive Kotlin App Development Reference Manual

**A detailed, app-focused guide to Kotlin for Android development, covering the language features, Android architecture, UI patterns, data layers, performance, testing, security, and production-ready workflows that matter when building real mobile applications.**

---

## Table of Contents
1. [Introduction to Kotlin App Development](#1-introduction-to-kotlin-app-development)
2. [Android Development Environment](#2-android-development-environment)
3. [Project Structure in a Kotlin Android App](#3-project-structure-in-a-kotlin-android-app)
4. [Kotlin Essentials for App Development](#4-kotlin-essentials-for-app-development)
5. [Object-Oriented Kotlin in Android](#5-object-oriented-kotlin-in-android)
6. [Null Safety and Reliability](#6-null-safety-and-reliability)
7. [Collections and Functional Patterns for UI/Data](#7-collections-and-functional-patterns-for-uidata)
8. [Android App Components](#8-android-app-components)
9. [Activity and Fragment Lifecycle](#9-activity-and-fragment-lifecycle)
10. [Jetpack Compose UI Development](#10-jetpack-compose-ui-development)
11. [Traditional XML Views and ViewBinding](#11-traditional-xml-views-and-viewbinding)
12. [Navigation and App Flow](#12-navigation-and-app-flow)
13. [App Architecture and Layering](#13-app-architecture-and-layering)
14. [State Management in Android Apps](#14-state-management-in-android-apps)
15. [Coroutines and Asynchronous Work](#15-coroutines-and-asynchronous-work)
16. [Networking in Kotlin Android Apps](#16-networking-in-kotlin-android-apps)
17. [Local Data Storage](#17-local-data-storage)
18. [Dependency Injection](#18-dependency-injection)
19. [Background Tasks and Long-running Work](#19-background-tasks-and-long-running-work)
20. [Permissions and Device Features](#20-permissions-and-device-features)
21. [Media, Images, and File Handling](#21-media-images-and-file-handling)
22. [Testing Android Apps](#22-testing-android-apps)
23. [Debugging and Performance Optimization](#23-debugging-and-performance-optimization)
24. [Security and Production Readiness](#24-security-and-production-readiness)
25. [Publishing and Maintenance](#25-publishing-and-maintenance)
26. [Best Practices](#26-best-practices)
27. [Common Mistakes](#27-common-mistakes)
28. [Real-world Examples](#28-real-world-examples)

---

## 1. Introduction to Kotlin App Development

### What Kotlin Means in App Development
Kotlin is a modern, expressive programming language officially supported for Android development. In app development, Kotlin is not just a language choice; it shapes how you structure UI, manage state, handle background work, and model data safely.

Kotlin is widely preferred in Android apps because it is:
- **Concise:** Reduces boilerplate compared with Java.
- **Safe:** Null safety reduces a huge category of crashes.
- **Interoperable:** Works smoothly with existing Java Android libraries.
- **Tool-friendly:** Android Studio provides strong autocomplete, refactoring, linting, and debugging support.
- **Coroutine-ready:** Makes asynchronous work much easier to read and manage.

### Why Kotlin Became the Default for Android
Android originally relied heavily on Java. Kotlin changed the developer experience by making common Android tasks easier:
- Creating immutable data models with `data class`
- Avoiding repetitive getters, setters, and constructors
- Writing safer code around nullable values
- Managing concurrency with coroutines instead of complex callback chains
- Building declarative UI with Jetpack Compose

### Scope of This Guide
This handbook stays focused on **Kotlin for app development**, especially Android app development. That means:
- We include language features only when they matter in mobile app work.
- We focus on UI, lifecycle, architecture, storage, networking, testing, and release practices.
- We do not spend time on unrelated Kotlin topics like server-side Ktor architecture, compiler internals, or competitive programming tricks unless they directly support mobile apps.

### Typical Modern Android Stack
A modern Kotlin Android app often includes:
1. **Kotlin** for language and business logic
2. **Jetpack Compose** or **XML Views** for UI
3. **ViewModel** for UI-related state
4. **Coroutines + Flow** for async streams
5. **Room** or **DataStore** for local persistence
6. **Retrofit** or another HTTP client for networking
7. **Hilt** for dependency injection
8. **Navigation** for screen flow
9. **WorkManager** for reliable background work
10. **JUnit / Mocking / UI tests** for quality assurance

---

## 2. Android Development Environment

### Android Studio
Android Studio is the primary IDE for Kotlin Android development. It bundles:
- Code editor with Kotlin support
- Layout tools
- Emulator management
- APK/App Bundle build tools
- Profilers
- Logcat
- Device management
- Gradle integration

### SDK, Emulator, and Devices
To build and test Android apps, you typically use:
- **Android SDK:** Provides APIs, platform tools, and build tools.
- **AVD Emulator:** Simulated Android devices for local testing.
- **Physical Device Testing:** Important for camera, sensors, performance, battery behavior, and real-world rendering.

### Gradle in Android Projects
Android projects are usually built with Gradle. It handles:
- Dependency resolution
- Build variants
- Signing
- Code generation
- Plugin configuration
- Test execution

Common Android build files include:
- `settings.gradle.kts`
- `build.gradle.kts` at project level
- `app/build.gradle.kts` at module level
- `gradle.properties`

### Build Variants
Android projects often define multiple variants:
- `debug`: Used during development
- `release`: Optimized, signed builds for publishing
- product flavors such as `dev`, `staging`, `prod`

This is useful when you need:
- Different API base URLs
- Feature flags
- Internal testing builds
- Separate app icons or labels

### Example Module Gradle File
```kotlin
plugins {
    id("com.android.application")
    id("org.jetbrains.kotlin.android")
}

android {
    namespace = "com.example.notesapp"
    compileSdk = 34

    defaultConfig {
        applicationId = "com.example.notesapp"
        minSdk = 24
        targetSdk = 34
        versionCode = 1
        versionName = "1.0"
    }

    buildTypes {
        release {
            isMinifyEnabled = true
        }
    }
}

dependencies {
    implementation("androidx.core:core-ktx:1.x.x")
    implementation("androidx.lifecycle:lifecycle-runtime-ktx:2.x.x")
}
```

### Tooling Mindset
Good Android development is not only about writing Kotlin code. You also need to be comfortable with:
- reading Gradle sync errors
- using the emulator
- checking manifests
- inspecting build outputs
- reading stack traces
- profiling memory and CPU

---

## 3. Project Structure in a Kotlin Android App

### Typical Android Module Layout
A standard app module often looks like:

```text
app/
  src/
    main/
      java/com/example/app/
      res/
      AndroidManifest.xml
    test/
    androidTest/
  build.gradle.kts
```

### Important Folders
- `java/` or `kotlin/`: Kotlin source files
- `res/`: Resources like layouts, strings, colors, drawables, fonts
- `AndroidManifest.xml`: App metadata, permissions, components
- `test/`: Local JVM unit tests
- `androidTest/`: Instrumented tests running on device/emulator

### Organizing Source Code by Feature
Instead of a flat package structure, app code is easier to maintain when organized by feature or layer.

Possible layout:

```text
com.example.app
  data/
  domain/
  ui/
  navigation/
  di/
  core/
```

Feature-first layout:

```text
com.example.app
  feature
    home/
    auth/
    profile/
  core/
  data/
```

### Recommended Separation
- **UI layer:** Composables, Activities, Fragments, screen state
- **Domain layer:** Use cases, business rules, pure models
- **Data layer:** Repositories, API services, database access, mappers
- **Core/shared:** Utilities, extensions, common result wrappers

### Resources Matter in Android
Unlike pure backend Kotlin, Android apps rely heavily on resources:
- `strings.xml` for user-facing text
- `colors.xml` for palettes
- `themes.xml` for styling
- `drawable/` for icons and shapes
- `mipmap/` for launcher icons
- `font/` for custom typography

### Manifest Responsibilities
The `AndroidManifest.xml` declares:
- application class
- launcher activity
- permissions
- services
- broadcast receivers
- deep links
- metadata

Example:
```xml
<manifest package="com.example.notesapp">
    <uses-permission android:name="android.permission.INTERNET" />

    <application>
        <activity
            android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>
</manifest>
```

---

## 4. Kotlin Essentials for App Development

### `val` vs `var`
In Android apps, immutability reduces bugs.

- `val`: read-only reference
- `var`: mutable reference

```kotlin
val appName = "Task Tracker"
var selectedTab = 0
```

Prefer `val` unless mutation is required.

### Basic Types You Use Constantly in Apps
- `String` for labels, IDs, URLs
- `Int` for counts, positions, resource IDs
- `Long` for timestamps
- `Boolean` for UI flags and feature toggles
- `Double` / `Float` for calculations and graphics
- `List<T>` for rendering collections on screen

### String Templates
Useful for UI text and logging.

```kotlin
val username = "Asha"
val greeting = "Welcome back, $username"
```

### `when` Expressions
`when` is extremely useful for:
- rendering UI based on state
- mapping API responses
- switching over sealed classes

```kotlin
fun statusText(isLoading: Boolean, hasError: Boolean): String {
    return when {
        isLoading -> "Loading..."
        hasError -> "Something went wrong"
        else -> "Ready"
    }
}
```

### Functions
Functions in Android apps often represent:
- event handlers
- validation logic
- mapping transformations
- use cases
- repository actions

```kotlin
fun formatUsername(name: String): String {
    return name.trim().replaceFirstChar { it.uppercase() }
}
```

### Default Arguments
Helpful for reusable UI and helper functions.

```kotlin
fun showToast(message: String, duration: Int = Toast.LENGTH_SHORT) {
    // usage inside an Android context
}
```

### Named Arguments
Improve readability in UI-heavy code.

```kotlin
Button(
    onClick = { /* save */ },
    enabled = true
) {
    Text("Save")
}
```

### Top-level Functions
Kotlin allows top-level functions outside classes. This is often useful for:
- formatting helpers
- extensions
- small mappers

### Data Classes
Android apps constantly move data between layers. `data class` is perfect for this.

```kotlin
data class User(
    val id: String,
    val name: String,
    val email: String
)
```

Benefits:
- auto-generated `equals()`
- `hashCode()`
- `toString()`
- `copy()`

### Copying Immutable State
Very common in Compose and ViewModel state updates.

```kotlin
data class ProfileUiState(
    val name: String = "",
    val isLoading: Boolean = false
)

val updated = state.copy(isLoading = true)
```

### Enums
Useful for fixed value sets such as sort order, theme mode, or screen tabs.

```kotlin
enum class ThemeMode {
    LIGHT, DARK, SYSTEM
}
```

### Sealed Classes
Ideal for app states and one-off UI events.

```kotlin
sealed class LoginResult {
    data object Success : LoginResult()
    data class Failure(val message: String) : LoginResult()
}
```

### Extension Functions
Used heavily in Android for cleaner code.

```kotlin
fun String.isValidEmail(): Boolean {
    return android.util.Patterns.EMAIL_ADDRESS.matcher(this).matches()
}
```

### Scope Functions
Kotlin scope functions are useful, but should be used intentionally:
- `let`
- `run`
- `apply`
- `also`
- `with`

Example:
```kotlin
val paint = Paint().apply {
    color = Color.BLUE
    strokeWidth = 4f
}
```

In app code:
- `apply` is common for object configuration
- `let` is common for null checks
- `also` is useful for side effects like logging

---

## 5. Object-Oriented Kotlin in Android

### Classes in App Code
You will create classes for:
- `Activity`
- `Fragment`
- `ViewModel`
- repositories
- database entities
- adapters
- use cases

### Constructors
Primary constructors keep model definitions concise.

```kotlin
class SessionManager(
    private val preferences: SharedPreferences
)
```

### Visibility Modifiers
Android apps benefit from limiting access properly:
- `public`: default visibility
- `private`: visible only inside the file/class
- `internal`: visible inside the module
- `protected`: visible in class hierarchy

Prefer the smallest visibility needed.

### Inheritance
Some Android APIs rely on inheritance:
- `ComponentActivity`
- `Fragment`
- `ViewModel`
- `RecyclerView.Adapter`

Example:
```kotlin
class MainActivity : ComponentActivity()
```

### Interfaces
Useful for contracts between layers.

```kotlin
interface AuthRepository {
    suspend fun login(email: String, password: String): Result<Unit>
}
```

### Abstract Classes vs Interfaces
- Use **interfaces** for capability contracts.
- Use **abstract classes** when you need shared implementation and state.

### Composition Over Inheritance
In app architecture, composition is usually cleaner.

Instead of:
- giant base fragments
- giant base activities

Prefer:
- reusable helper classes
- delegated state holders
- shared composables
- injectable dependencies

### Delegation
Kotlin delegation can reduce Android boilerplate.

Examples:
- property delegates like `by lazy`
- Compose state delegates like `by remember`
- interface delegation in advanced cases

```kotlin
val repository by lazy {
    UserRepository(apiService)
}
```

### Companion Objects
Useful for factory methods or constants.

```kotlin
class DetailActivity : AppCompatActivity() {
    companion object {
        const val EXTRA_USER_ID = "extra_user_id"
    }
}
```

### Parcelable Models
Android often needs object transfer between screens or process boundaries.

```kotlin
@Parcelize
data class Article(
    val id: String,
    val title: String
) : Parcelable
```

Use parcelable models carefully:
- keep them small
- avoid large nested graphs
- prefer IDs over huge objects where possible

---

## 6. Null Safety and Reliability

### Why Null Safety Matters in Apps
Mobile apps crash in users’ hands, not just on developer machines. Kotlin’s null safety helps reduce runtime crashes caused by unexpected missing values.

### Nullable Types
```kotlin
val email: String? = null
```

This tells the compiler that `email` may be absent.

### Safe Call Operator
```kotlin
val length = email?.length
```

If `email` is null, the expression returns null instead of crashing.

### Elvis Operator
Very useful for UI defaults.

```kotlin
val title = article.title ?: "Untitled"
```

### `let` for Nullable Values
```kotlin
user?.let {
    showProfile(it)
}
```

### Non-null Assertion `!!`
```kotlin
val name = user!!.name
```

This forces a crash if `user` is null.

Avoid `!!` in production app code unless:
- you truly control the invariant
- the crash is better than silent corruption

### Late Initialization
You may see:
```kotlin
lateinit var binding: ActivityMainBinding
```

This is common in Android Views, but should be used carefully.

Risks:
- accessing before initialization causes crash
- harder to reason about than constructor injection

### Nullable UI State
Instead of:
```kotlin
val user: User? = null
```

Consider explicit state modeling:
```kotlin
sealed interface ProfileState {
    data object Loading : ProfileState
    data class Success(val user: User) : ProfileState
    data class Error(val message: String) : ProfileState
}
```

This is usually clearer than null-based logic.

### Defensive App Coding
Good null handling in apps includes:
- validating intent extras
- handling missing network fields
- checking permissions results
- guarding against lifecycle timing issues
- assuming disk and network data can be incomplete

---

## 7. Collections and Functional Patterns for UI/Data

### Lists Drive Most Screens
Many app screens render collections:
- feed items
- chat messages
- notifications
- settings rows
- search results

Kotlin collection APIs make these transformations concise.

### Common Collection Operations
```kotlin
val products = listOf("Phone", "Tablet", "Laptop")
val longNames = products.filter { it.length > 5 }
val upper = products.map { it.uppercase() }
```

### `map`
Used constantly for:
- DTO to domain model mapping
- domain to UI model conversion
- database entity transformation

```kotlin
val uiItems = users.map { user ->
    UserCardModel(
        id = user.id,
        title = user.name,
        subtitle = user.email
    )
}
```

### `filter`
Useful for:
- search
- category tabs
- local data constraints

### `firstOrNull`
Safer than assuming a match exists.

```kotlin
val admin = users.firstOrNull { it.role == "admin" }
```

### `groupBy`
Helpful for sectioned lists like message dates or categories.

```kotlin
val grouped = transactions.groupBy { it.date }
```

### `sortedBy`
Used before rendering.

```kotlin
val newest = messages.sortedByDescending { it.timestamp }
```

### Immutable Collections
Prefer returning immutable lists from repositories and state holders. Mutable collections can lead to hidden UI inconsistencies.

### Functional UI Thinking
In modern Android, especially Compose, you often transform data like this:
1. fetch raw data
2. map to UI state
3. render based on current state
4. emit user actions upward

This makes app behavior more predictable.

### Example Mapping Chain
```kotlin
val visibleItems = allItems
    .filter { it.isVisible }
    .sortedBy { it.title }
    .map { item -> item.toCardModel() }
```

### Beware of Over-chaining
Chaining is expressive, but do not make transformations unreadable. In hot paths, consider performance and intermediate allocations.

---

## 8. Android App Components

### Activities
An `Activity` represents a major screen container and entry point into the app.

Use activities for:
- app launch
- hosting Compose content
- screen containers
- system-facing integrations

### Fragments
Fragments are reusable UI/controller components that live inside activities. Many existing apps still use them heavily.

Use fragments when:
- working in an existing XML/Fragment codebase
- integrating with Navigation Component fragment graphs
- reusing screen sections in classic architecture

### Services
Services are used for work that needs to continue outside a visible UI.

Common use cases:
- media playback
- foreground tracking
- system-integrated background behavior

### Broadcast Receivers
Broadcast receivers listen to system or app broadcasts.

Examples:
- boot completed
- connectivity changes
- custom app events

### Content Providers
Less common in typical app code, but important for:
- structured data sharing between apps
- exposing app data safely

### Application Class
The `Application` class is created when the app process starts.

Used for:
- app-wide initialization
- DI container setup
- logging or analytics bootstrapping

Do not turn it into a global dumping ground.

### Intents
Intents are message objects used to:
- launch activities
- start services
- share data
- open external apps

Example:
```kotlin
val intent = Intent(this, DetailActivity::class.java)
intent.putExtra("item_id", "42")
startActivity(intent)
```

### PendingIntent
Used when another process or system service needs permission to execute an intent later.

Common with:
- notifications
- alarms
- widgets

---

## 9. Activity and Fragment Lifecycle

### Why Lifecycle Knowledge Is Critical
Android app components are managed by the system. If you do not understand lifecycle events, you get:
- leaks
- crashes
- duplicate requests
- lost UI state
- wasted work

### Activity Lifecycle
Important callbacks:
- `onCreate()`
- `onStart()`
- `onResume()`
- `onPause()`
- `onStop()`
- `onDestroy()`

High-level flow:
1. `onCreate()` initializes the screen
2. `onStart()` makes it visible
3. `onResume()` makes it interactive
4. `onPause()` loses focus
5. `onStop()` is no longer visible
6. `onDestroy()` cleans up final resources

### Fragment Lifecycle Nuance
Fragments have both:
- fragment lifecycle
- view lifecycle

This matters because the fragment may exist while its view is destroyed and recreated.

### ViewBinding in Fragments
If using ViewBinding in fragments, you usually clear binding in `onDestroyView()` to avoid leaks.

```kotlin
private var _binding: FragmentHomeBinding? = null
private val binding get() = _binding!!

override fun onCreateView(
    inflater: LayoutInflater,
    container: ViewGroup?,
    savedInstanceState: Bundle?
): View {
    _binding = FragmentHomeBinding.inflate(inflater, container, false)
    return binding.root
}

override fun onDestroyView() {
    super.onDestroyView()
    _binding = null
}
```

### Lifecycle-aware Collection
When collecting Flows in UI, use lifecycle-aware APIs so work pauses when the screen is not visible.

```kotlin
lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.uiState.collect { state ->
            render(state)
        }
    }
}
```

### Configuration Changes
Rotation, language change, or window changes can recreate activities.

To survive configuration changes:
- keep UI state in `ViewModel`
- persist important data in `SavedStateHandle`
- avoid storing UI-only state inside activities/fragments

### Process Death
Android may kill your app process to reclaim memory.

Good apps recover by:
- persisting essential state
- reloading from repositories or database
- handling missing in-memory data gracefully

---

## 10. Jetpack Compose UI Development

### What Compose Is
Jetpack Compose is Android’s declarative UI toolkit. Instead of imperatively mutating views, you describe the UI for the current state.

### Core Idea
UI = function(state)

If state changes, Compose recomposes the affected UI.

### Basic Composable
```kotlin
@Composable
fun Greeting(name: String) {
    Text(text = "Hello, $name")
}
```

### Setting Compose Content in an Activity
```kotlin
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MaterialTheme {
                Greeting(name = "Developer")
            }
        }
    }
}
```

### State in Compose
Compose observes state and updates UI when it changes.

```kotlin
@Composable
fun CounterScreen() {
    var count by remember { mutableStateOf(0) }

    Column {
        Text("Count: $count")
        Button(onClick = { count++ }) {
            Text("Increment")
        }
    }
}
```

### `remember`
Keeps a value across recompositions in the same composition.

### `rememberSaveable`
Keeps small UI state across configuration changes when possible.

```kotlin
var query by rememberSaveable { mutableStateOf("") }
```

### State Hoisting
A key Compose pattern:
- state should often live higher up
- child composables receive values and callbacks

```kotlin
@Composable
fun SearchBar(
    query: String,
    onQueryChange: (String) -> Unit
) {
    OutlinedTextField(
        value = query,
        onValueChange = onQueryChange,
        label = { Text("Search") }
    )
}
```

### Recomposition Awareness
Compose may re-run composable functions frequently.

This means:
- avoid side effects directly in composable bodies
- keep expensive work outside recomposition
- prefer stable state flows

### Lists in Compose
Use `LazyColumn` for efficient scrolling lists.

```kotlin
LazyColumn {
    items(messages, key = { it.id }) { message ->
        Text(message.text)
    }
}
```

### Side Effects
Important Compose side-effect APIs:
- `LaunchedEffect`
- `DisposableEffect`
- `rememberCoroutineScope`
- `SideEffect`
- `produceState`

Example:
```kotlin
LaunchedEffect(userId) {
    viewModel.loadUser(userId)
}
```

### Material Design
Compose integrates well with Material components:
- `Scaffold`
- `TopAppBar`
- `Button`
- `Card`
- `Snackbar`
- `NavigationBar`

### Scaffold
Often used as the base screen layout.

```kotlin
Scaffold(
    topBar = { TopAppBar(title = { Text("Home") }) }
) { padding ->
    Box(modifier = Modifier.padding(padding)) {
        Text("Content")
    }
}
```

### Theming
Compose themes usually define:
- color scheme
- typography
- shapes

Centralizing theme values keeps UI consistent across screens.

### Preview
`@Preview` helps iterate quickly without launching the full app.

```kotlin
@Preview(showBackground = true)
@Composable
fun GreetingPreview() {
    Greeting("Preview")
}
```

### Compose in Production
Compose works best when you:
- model UI as immutable state
- use stable keys in lists
- avoid business logic inside composables
- collect ViewModel state safely

---

## 11. Traditional XML Views and ViewBinding

### Why This Still Matters
Many Android apps still use:
- XML layouts
- Fragments
- RecyclerView
- custom Views

Even in new apps, you may need interoperability with existing UI systems.

### XML Layout Basics
Layouts are defined in `res/layout`.

Example:
```xml
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <TextView
        android:id="@+id/titleText"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello" />

</LinearLayout>
```

### ViewBinding
ViewBinding generates type-safe access to views.

Activity example:
```kotlin
class MainActivity : AppCompatActivity() {
    private lateinit var binding: ActivityMainBinding

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)

        binding.titleText.text = "Welcome"
    }
}
```

### Why ViewBinding Is Better Than `findViewById`
- type-safe
- less boilerplate
- fewer null mistakes
- cleaner code

### RecyclerView
RecyclerView remains important in View-based apps for efficient list rendering.

Core parts:
- Adapter
- ViewHolder
- LayoutManager

### Adapter Example
```kotlin
class MessageAdapter(
    private val items: List<Message>
) : RecyclerView.Adapter<MessageAdapter.MessageViewHolder>() {

    class MessageViewHolder(val binding: ItemMessageBinding) :
        RecyclerView.ViewHolder(binding.root)

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): MessageViewHolder {
        val inflater = LayoutInflater.from(parent.context)
        val binding = ItemMessageBinding.inflate(inflater, parent, false)
        return MessageViewHolder(binding)
    }

    override fun onBindViewHolder(holder: MessageViewHolder, position: Int) {
        holder.binding.messageText.text = items[position].text
    }

    override fun getItemCount(): Int = items.size
}
```

### Compose + View Interop
You can migrate gradually.

Examples:
- host Compose inside fragment/activity
- embed existing views in Compose with `AndroidView`
- keep legacy screens while new screens use Compose

---

## 12. Navigation and App Flow

### Screen Navigation Basics
App navigation is about moving between destinations while preserving data, back stack behavior, and deep links.

### Common Patterns
- single-activity apps with Compose navigation
- fragment-based navigation graph apps
- bottom navigation for top-level sections
- nested graphs for auth or onboarding

### Compose Navigation
Compose apps often use a `NavHost`.

```kotlin
NavHost(
    navController = navController,
    startDestination = "home"
) {
    composable("home") { HomeScreen(onOpenProfile = { userId ->
        navController.navigate("profile/$userId")
    }) }

    composable("profile/{userId}") { backStackEntry ->
        val userId = backStackEntry.arguments?.getString("userId").orEmpty()
        ProfileScreen(userId = userId)
    }
}
```

### Passing Data Between Screens
Prefer passing:
- IDs
- lightweight route arguments

Avoid passing:
- giant object graphs
- mutable shared references

### Deep Links
Deep links let external URLs or app links open specific screens. They are common for:
- product links
- email verification
- reset password
- promotional campaigns

### Back Stack Behavior
Users expect the back button to feel natural. Think about:
- should a screen be popped or replaced?
- should login clear prior screens?
- should bottom tabs restore separate stacks?

### Navigation Events
In MVVM-style apps, navigation often originates from UI events or ViewModel-emitted commands. Keep navigation decisions predictable and avoid firing them repeatedly during recomposition.

---

## 13. App Architecture and Layering

### Why Architecture Matters
As app complexity grows, architecture keeps code:
- testable
- scalable
- reusable
- understandable

### Common Layered Structure
1. **UI layer**
2. **Domain layer**
3. **Data layer**

### UI Layer
Contains:
- composables or fragments
- screen state
- event handlers
- ViewModels

Responsibilities:
- render state
- collect user input
- delegate business actions

### Domain Layer
Optional for small apps, but useful in medium/large apps.

Contains:
- use cases
- business rules
- pure models

Example:
```kotlin
class LoginUseCase(
    private val repository: AuthRepository
) {
    suspend operator fun invoke(email: String, password: String): Result<Unit> {
        return repository.login(email, password)
    }
}
```

### Data Layer
Contains:
- repositories
- remote data sources
- local data sources
- DTOs/entities
- mappers

### Repository Pattern
Repositories abstract where data comes from.

```kotlin
class UserRepository(
    private val api: UserApi,
    private val dao: UserDao
) {
    suspend fun getUsers(): List<User> {
        val remote = api.fetchUsers()
        dao.insertAll(remote.map { it.toEntity() })
        return dao.getAll().map { it.toDomain() }
    }
}
```

### MVVM
Model-View-ViewModel is very common in Android.

- **View:** Compose UI or Fragment
- **ViewModel:** holds UI state and logic
- **Model:** domain/data layer entities and operations

Benefits:
- lifecycle-aware state holding
- cleaner separation
- easier testing

### Unidirectional Data Flow
A strong pattern for predictable apps:
1. UI emits event
2. ViewModel handles event
3. ViewModel updates state
4. UI re-renders from new state

This works especially well with Compose.

### Keep Layers Honest
Avoid:
- networking inside composables
- database access inside activities
- Android `Context` leaking into domain code

---

## 14. State Management in Android Apps

### UI State Is Central
Modern Android apps are primarily state machines with a UI. State management determines whether the app feels stable or chaotic.

### Types of State
- screen state
- transient UI state
- persisted state
- background/loading state
- error state

### Single Screen State Object
A common pattern:
```kotlin
data class HomeUiState(
    val isLoading: Boolean = false,
    val items: List<TaskItem> = emptyList(),
    val errorMessage: String? = null
)
```

This keeps rendering simple.

### Why a Single State Object Helps
- easier previews
- easier testing
- fewer inconsistent partial updates
- clearer recomposition boundaries

### StateFlow
`StateFlow` is a common way to expose observable UI state from a ViewModel.

```kotlin
class HomeViewModel : ViewModel() {
    private val _uiState = MutableStateFlow(HomeUiState())
    val uiState: StateFlow<HomeUiState> = _uiState
}
```

### Updating State
```kotlin
_uiState.update { current ->
    current.copy(isLoading = true)
}
```

### One-time Events
Be careful with snackbars, navigation, and toasts. These are events, not persistent state.

Options include:
- `SharedFlow`
- channels
- explicit event wrappers
- UI callback consumption

### SavedStateHandle
Useful in ViewModels for state that should survive process recreation.

Examples:
- selected item ID
- active filter
- search query

### Compose Local State vs ViewModel State
Use local Compose state for:
- text field temporary values
- dialog open/closed
- tab selection limited to current screen

Use ViewModel state for:
- business-relevant screen data
- repository-backed values
- state that must survive configuration changes

---

## 15. Coroutines and Asynchronous Work

### Why Coroutines Matter
Apps constantly do asynchronous work:
- API requests
- database calls
- image loading
- background sync
- timers
- animations

Coroutines make this manageable without callback pyramids.

### `suspend` Functions
A `suspend` function can pause without blocking the thread.

```kotlin
suspend fun fetchProfile(): Profile {
    return api.getProfile()
}
```

### Launching Coroutines in ViewModel
```kotlin
class ProfileViewModel(
    private val repository: ProfileRepository
) : ViewModel() {

    fun loadProfile() {
        viewModelScope.launch {
            // async work here
        }
    }
}
```

### Dispatchers
Common dispatchers:
- `Dispatchers.Main` for UI work
- `Dispatchers.IO` for blocking I/O
- `Dispatchers.Default` for CPU-intensive tasks

Example:
```kotlin
viewModelScope.launch {
    val result = withContext(Dispatchers.IO) {
        repository.loadData()
    }
}
```

### Structured Concurrency
Coroutines are easier to reason about when their lifecycle is tied to a scope:
- `viewModelScope`
- `lifecycleScope`
- custom scopes in data layers when appropriate

This helps prevent orphaned work.

### Error Handling in Coroutines
Use `try/catch` inside coroutines.

```kotlin
viewModelScope.launch {
    try {
        val users = repository.getUsers()
        _uiState.update { it.copy(items = users, isLoading = false) }
    } catch (e: Exception) {
        _uiState.update { it.copy(errorMessage = e.message, isLoading = false) }
    }
}
```

### Cancellation
Coroutines are cooperative. If a screen leaves scope, its coroutines may be cancelled. Avoid swallowing `CancellationException` unintentionally.

### Parallel Work
Use `async` when tasks are independent and you truly need concurrency.

```kotlin
viewModelScope.launch {
    val profileDeferred = async { repository.getProfile() }
    val postsDeferred = async { repository.getPosts() }

    val profile = profileDeferred.await()
    val posts = postsDeferred.await()
}
```

### Flow
`Flow` represents a stream of asynchronous values.

Useful for:
- database observation
- search queries
- data sync streams
- reactive UI state pipelines

Example:
```kotlin
fun observeTasks(): Flow<List<Task>> = taskDao.observeAll()
```

### StateFlow vs SharedFlow
- `StateFlow`: latest state, always has a current value
- `SharedFlow`: broadcasts events or streams to collectors

### Combining Flows
```kotlin
val screenState = combine(tasksFlow, queryFlow) { tasks, query ->
    tasks.filter { it.title.contains(query, ignoreCase = true) }
}
```

### Debouncing User Input
Great for search fields:
- avoid sending API requests for every keystroke
- reduce jitter and network waste

### Coroutines Best Uses in Apps
- fetch data without freezing UI
- observe database updates
- run sync tasks safely
- cancel work when the screen disappears

---

## 16. Networking in Kotlin Android Apps

### Networking Basics
Most apps talk to remote services for:
- login
- content feeds
- search
- uploads
- sync
- analytics

### Retrofit
Retrofit is a common HTTP client abstraction on Android.

Example interface:
```kotlin
interface ArticleApi {
    @GET("articles")
    suspend fun getArticles(): List<ArticleDto>

    @POST("login")
    suspend fun login(@Body request: LoginRequest): LoginResponse
}
```

### DTOs
Use dedicated DTOs for API contracts.

```kotlin
data class ArticleDto(
    val id: String,
    val title: String,
    val summary: String?
)
```

Avoid using DTOs directly in the UI layer.

### Mapping Network Models
```kotlin
fun ArticleDto.toDomain(): Article {
    return Article(
        id = id,
        title = title,
        summary = summary ?: ""
    )
}
```

### Error Handling Strategy
Network requests can fail because of:
- no internet
- timeout
- server errors
- invalid JSON
- authentication failures

Design a predictable result model.

Example:
```kotlin
sealed interface AppResult<out T> {
    data class Success<T>(val data: T) : AppResult<T>
    data class Error(val message: String) : AppResult<Nothing>
}
```

### Repository-level Handling
```kotlin
suspend fun loadArticles(): AppResult<List<Article>> {
    return try {
        val data = api.getArticles().map { it.toDomain() }
        AppResult.Success(data)
    } catch (e: Exception) {
        AppResult.Error(e.message ?: "Unknown network error")
    }
}
```

### Authentication
Apps commonly deal with:
- bearer tokens
- refresh tokens
- session expiry
- logout on unauthorized responses

You often centralize token handling in interceptors or authenticated data sources.

### Pagination
Large feeds often load data page by page. Keep pagination logic outside the UI where possible.

### Offline-first Thinking
Production apps often combine:
- local cache
- network refresh
- stale data rendering
- background sync

This usually feels better than always waiting on fresh network responses.

---

## 17. Local Data Storage

### Why Local Storage Matters
Apps need local storage for:
- caching
- offline access
- saved preferences
- drafts
- tokens
- user settings

### Room Database
Room is a SQLite abstraction for Android.

Main parts:
- `@Entity`
- `@Dao`
- `@Database`

Entity:
```kotlin
@Entity(tableName = "notes")
data class NoteEntity(
    @PrimaryKey val id: String,
    val title: String,
    val content: String,
    val updatedAt: Long
)
```

DAO:
```kotlin
@Dao
interface NoteDao {
    @Query("SELECT * FROM notes ORDER BY updatedAt DESC")
    fun observeAll(): Flow<List<NoteEntity>>

    @Insert(onConflict = OnConflictStrategy.REPLACE)
    suspend fun insertAll(items: List<NoteEntity>)
}
```

Database:
```kotlin
@Database(entities = [NoteEntity::class], version = 1)
abstract class AppDatabase : RoomDatabase() {
    abstract fun noteDao(): NoteDao
}
```

### Room Benefits
- compile-time SQL validation
- Flow integration
- structured persistence
- clear entity/DAO separation

### DataStore
Use DataStore for lightweight key-value or typed preferences.

Good for:
- theme mode
- onboarding completion flag
- small settings

Better than old `SharedPreferences` for many cases.

### File Storage
Useful for:
- exported documents
- cached media
- app-generated files

Be mindful of:
- scoped storage rules
- file size
- permissions
- cleanup strategy

### Caching Strategy
Common local-first approaches:
1. show cached data immediately
2. refresh from network
3. update database
4. UI reacts automatically from Flow

### Migrations
When the schema changes, Room migrations matter. Ignoring them can cause crashes or data loss in real users’ devices.

---

## 18. Dependency Injection

### Why Dependency Injection Matters
Dependency injection helps you:
- remove hard-coded object creation
- improve testing
- share app-wide services cleanly
- reduce tight coupling

### Common DI Choices
- Hilt
- Dagger
- Koin
- manual dependency injection

### Hilt Conceptually
With Hilt, you define:
- injectable classes
- modules for object providers
- Android entry points

Example:
```kotlin
@HiltAndroidApp
class MyApp : Application()
```

Providing a dependency:
```kotlin
@Module
@InstallIn(SingletonComponent::class)
object NetworkModule {

    @Provides
    fun provideApiService(): ArticleApi {
        TODO("Create Retrofit service")
    }
}
```

Injecting into a ViewModel:
```kotlin
@HiltViewModel
class HomeViewModel @Inject constructor(
    private val repository: ArticleRepository
) : ViewModel()
```

### Manual DI Is Still Valid
In small apps, manual dependency passing can be enough. The key principle is still the same:
- create dependencies centrally
- pass them where needed
- avoid hidden globals

### DI Best Practices
- inject interfaces where useful
- keep modules focused
- avoid service locator anti-patterns
- do not inject everything everywhere without thought

---

## 19. Background Tasks and Long-running Work

### Background Work Types
Android background work varies a lot:
- guaranteed deferrable work
- immediate foreground work
- periodic sync
- alarms
- media playback

### WorkManager
Use WorkManager for reliable deferrable tasks that should run even if the app exits.

Good examples:
- upload retry
- sync unsent data
- cleanup old files
- periodic refresh

Worker example:
```kotlin
class SyncWorker(
    appContext: Context,
    params: WorkerParameters
) : CoroutineWorker(appContext, params) {

    override suspend fun doWork(): Result {
        return try {
            // sync logic
            Result.success()
        } catch (e: Exception) {
            Result.retry()
        }
    }
}
```

Scheduling work:
```kotlin
val request = OneTimeWorkRequestBuilder<SyncWorker>().build()
WorkManager.getInstance(context).enqueue(request)
```

### Foreground Services
Needed when users are actively aware of ongoing work such as:
- navigation tracking
- active downloads
- media playback

Foreground services require visible notifications and careful platform compliance.

### Alarms
For exact timing, alarms may be used, but Android increasingly restricts battery-heavy patterns. Use them only when the use case truly needs exact timing.

### Background Limits
Android aggressively manages battery and background execution. Design background work with platform expectations in mind rather than fighting them.

---

## 20. Permissions and Device Features

### Runtime Permissions
Many device capabilities require runtime permission requests.

Common examples:
- camera
- location
- notifications
- microphone
- contacts
- storage-related access depending on API level

### Permission Flow
Good permission UX usually includes:
1. explain why the feature needs permission
2. request at the moment of need
3. handle denial gracefully
4. provide settings fallback if permanently denied

### Activity Result APIs
Modern Android permission requests often use activity result contracts.

```kotlin
val launcher = registerForActivityResult(
    ActivityResultContracts.RequestPermission()
) { isGranted ->
    if (isGranted) {
        startCamera()
    } else {
        showPermissionDeniedMessage()
    }
}
```

### Location
Location features involve:
- permission handling
- battery impact
- precision tradeoffs
- background policies

### Camera
Apps using camera need to think about:
- capture flow
- temporary file storage
- image compression
- orientation
- lifecycle cleanup

### Notifications
Notifications require:
- permission on newer Android versions
- sensible channels
- actionable content
- deep link behavior

### Sensors and Hardware
Some apps use:
- accelerometer
- gyroscope
- biometric authentication
- Bluetooth
- NFC

Always feature-detect rather than assuming hardware support.

---

## 21. Media, Images, and File Handling

### Image Loading
Most apps load remote images. Use dedicated libraries rather than manual bitmap management.

Important concerns:
- caching
- placeholders
- error states
- memory usage
- resizing

### User-selected Media
Apps often let users:
- pick images
- capture photos
- attach documents
- upload audio/video

Design the flow around:
- content URIs
- temporary access permissions
- safe copying when needed

### File Uploads
When uploading files:
- validate size
- show progress
- handle network failure
- retry carefully

### Media Playback
Apps with audio/video should think about:
- lifecycle pause/resume
- audio focus
- notification controls
- background behavior

### Large Files
Avoid reading huge files fully into memory when streaming approaches are better.

---

## 22. Testing Android Apps

### Why Testing Matters
Apps deal with many fragile conditions:
- lifecycle changes
- asynchronous work
- flaky networks
- state restoration
- device fragmentation

Testing reduces regressions dramatically.

### Test Types
- unit tests
- integration tests
- UI tests
- instrumentation tests

### Unit Tests
Unit tests are best for:
- use cases
- repository logic
- mappers
- validators
- ViewModel behavior

Example:
```kotlin
class EmailValidatorTest {

    @Test
    fun validEmail_returnsTrue() {
        val result = "hello@example.com".isValidEmail()
        assertTrue(result)
    }
}
```

### ViewModel Testing
ViewModels are great test targets because they contain UI logic without rendering concerns.

You often test:
- loading state
- success state
- error state
- event emission

### Fake Repositories
Prefer fakes or mocks for data dependencies. A fake repository is often simpler and more readable than over-mocking everything.

### Coroutine Testing
Coroutine-based code should use coroutine test utilities so timing and dispatchers remain deterministic.

### UI Testing
Compose UI tests or Espresso-style tests verify:
- screen rendering
- interaction flows
- navigation
- text visibility
- button actions

Compose test example conceptually:
```kotlin
composeTestRule
    .onNodeWithText("Login")
    .performClick()
```

### Instrumented Tests
These run on a device or emulator and are useful when Android framework behavior is part of what you must verify.

### Testing Strategy
A healthy app often has:
- many unit tests
- a moderate number of integration tests
- a focused set of UI tests for critical user journeys

---

## 23. Debugging and Performance Optimization

### Debugging Tools
Key Android debugging tools include:
- Logcat
- debugger breakpoints
- Layout Inspector
- Network Inspector
- Database Inspector
- Memory profiler
- CPU profiler

### Logging
Logging should help you answer:
- what happened
- when it happened
- what state led here
- which request failed

Avoid logging secrets or personal user data.

### Stack Traces
When reading Android crashes:
1. find the first relevant app-owned stack frame
2. identify nulls, bad indexes, illegal state, or threading issues
3. reproduce with the same lifecycle or state conditions

### ANRs
Application Not Responding errors usually happen when the main thread is blocked too long.

Causes include:
- heavy database work on main thread
- large JSON parsing on main thread
- slow startup
- blocking file I/O

### Smooth UI Matters
Performance issues users feel most:
- janky scrolling
- long startup
- laggy typing
- delayed navigation
- battery drain

### Performance Principles
- keep heavy work off main thread
- paginate large lists
- avoid unnecessary recompositions
- cache wisely
- measure before over-optimizing

### Compose-specific Performance Awareness
Watch for:
- unstable parameters
- excessive recompositions
- expensive work inside composables
- missing list keys

### Memory Leaks
Common leak sources:
- retaining context in singletons
- fragment view binding misuse
- long-lived listeners not removed
- static references to activities

### Startup Optimization
App startup improves when you:
- defer non-critical initialization
- avoid blocking splash flow
- lazily create expensive objects

---

## 24. Security and Production Readiness

### Mobile Security Basics
App security includes:
- protecting local data
- securing network traffic
- minimizing exposed attack surface
- avoiding secret leakage

### Never Hardcode Secrets
Do not hardcode:
- API secrets
- private keys
- admin credentials

Anything shipped in the client can potentially be extracted.

### Secure Network Practices
- use HTTPS
- validate auth flows
- handle token refresh safely
- avoid verbose error leaks

### Local Sensitive Data
Sensitive user data should be stored carefully. Tokens and credentials need stronger protection than casual preferences.

### Input Validation
Even client apps should validate:
- form fields
- file types
- lengths
- URLs

This improves UX and reduces bad data propagation.

### Release Build Hygiene
Before release:
- remove debug logs where appropriate
- verify signing setup
- check minification behavior
- confirm analytics and crash reporting are correct
- test release variants, not only debug builds

### Crash Monitoring
Production apps should have:
- crash reporting
- error aggregation
- version-aware diagnostics

This helps connect real user issues back to code changes.

---

## 25. Publishing and Maintenance

### Preparing for Release
Release readiness includes more than code compiling. You also need:
- versioning
- signing
- store metadata
- screenshots
- testing coverage
- privacy and permission review

### Versioning
Android apps typically use:
- `versionCode`: integer for upgrades
- `versionName`: user-visible version label

### App Bundle / APK
Modern Play Store distribution commonly uses app bundles for optimized delivery.

### Post-release Work
Shipping is not the end. Maintenance includes:
- crash triage
- analytics review
- dependency updates
- performance tuning
- OS compatibility testing

### Backward Compatibility
Android ecosystem diversity means you should test across:
- API levels
- screen sizes
- light/dark themes
- locale changes
- network conditions

### Documentation Helps Teams
Good app teams maintain:
- architecture notes
- setup instructions
- module responsibilities
- feature flags documentation
- release checklists

---

## 26. Best Practices

1. **Prefer `val` and immutable state whenever possible:**
   Immutable state is easier to reason about, especially in Compose and ViewModel-based apps.

2. **Model screen state explicitly:**
   Use data classes or sealed interfaces instead of scattered booleans across the UI layer.

3. **Keep business logic out of composables and activities:**
   UI should render and forward actions, not own complex application behavior.

4. **Use `ViewModel` for screen-level state:**
   This keeps state lifecycle-aware and resilient to configuration changes.

5. **Treat nullability seriously:**
   Avoid `!!` unless the invariant is truly guaranteed.

6. **Map data between layers:**
   Convert DTOs, entities, domain models, and UI models intentionally instead of letting layers bleed together.

7. **Use coroutines with structured concurrency:**
   Tie async work to scopes like `viewModelScope` and `lifecycleScope`.

8. **Collect streams with lifecycle awareness:**
   Prevent background UI work and reduce leaks or duplicate rendering.

9. **Favor unidirectional data flow:**
   Events go in, state comes out. This makes behavior predictable.

10. **Cache and persist thoughtfully:**
    Good apps degrade gracefully offline or during slow network conditions.

11. **Request permissions contextually:**
    Ask when the user understands why the feature needs access.

12. **Write tests for ViewModels, use cases, and mappers:**
    These layers usually provide the best return on testing effort.

13. **Profile before optimizing:**
    Use Android Studio tools instead of guessing.

14. **Design for process death and rotation:**
    Mobile apps run in constrained environments; assume recreation will happen.

15. **Keep modules and packages understandable:**
    Consistent structure helps future maintainers move faster.

16. **Prefer feature-based organization in larger apps:**
    This improves ownership and scaling over time.

17. **Use stable keys in list rendering:**
    Especially important in Compose or RecyclerView diffing scenarios.

18. **Validate release builds separately from debug:**
    Shrinking, obfuscation, and production config can reveal hidden issues.

---

## 27. Common Mistakes

1. **Putting network calls directly inside composables or activities:**
   This mixes UI and data concerns and often breaks lifecycle expectations.

2. **Using too many mutable variables:**
   Uncontrolled mutation makes state bugs much harder to diagnose.

3. **Overusing `!!`:**
   It defeats Kotlin’s safety advantages and can produce avoidable crashes.

4. **Ignoring fragment view lifecycle:**
   Holding view references after `onDestroyView()` often causes leaks.

5. **Collecting Flow without lifecycle awareness:**
   This can trigger duplicate collectors, wasted work, or crashes after screen transitions.

6. **Blocking the main thread:**
   Disk, network, and heavy computation on main thread cause jank or ANRs.

7. **Exposing mutable state publicly:**
   ViewModels should expose immutable `StateFlow` or read-only state to the UI.

8. **Using one model class for everything:**
   API, database, domain, and UI models often need different shapes and responsibilities.

9. **Treating events like persistent state:**
   Snackbars and navigation should not be stored forever in screen state without careful handling.

10. **Triggering side effects repeatedly during recomposition:**
    In Compose, side effects belong in proper effect APIs, not plain composable bodies.

11. **Passing huge objects through navigation:**
    Prefer lightweight IDs and reloading data in the destination when sensible.

12. **Neglecting offline and retry behavior:**
    Real mobile users lose connectivity often.

13. **Shipping debug assumptions into production:**
    Hardcoded endpoints, logs, or fake flags can leak into release if build discipline is weak.

14. **Treating all exceptions the same:**
    User messaging should differ for auth failure, no internet, timeout, or server errors.

15. **Not testing rotation, process recreation, and background return flows:**
    Many app bugs only appear outside the simplest happy path.

---

## 28. Real-world Examples

### Example 1: Compose Screen with ViewModel State

```kotlin
data class LoginUiState(
    val email: String = "",
    val password: String = "",
    val isLoading: Boolean = false,
    val errorMessage: String? = null
)

class LoginViewModel(
    private val loginUseCase: LoginUseCase
) : ViewModel() {

    private val _uiState = MutableStateFlow(LoginUiState())
    val uiState: StateFlow<LoginUiState> = _uiState

    fun onEmailChange(value: String) {
        _uiState.update { it.copy(email = value) }
    }

    fun onPasswordChange(value: String) {
        _uiState.update { it.copy(password = value) }
    }

    fun onLoginClick() {
        viewModelScope.launch {
            _uiState.update { it.copy(isLoading = true, errorMessage = null) }

            val state = _uiState.value
            val result = loginUseCase(state.email, state.password)

            _uiState.update {
                when {
                    result.isSuccess -> it.copy(isLoading = false)
                    else -> it.copy(
                        isLoading = false,
                        errorMessage = result.exceptionOrNull()?.message ?: "Login failed"
                    )
                }
            }
        }
    }
}

@Composable
fun LoginScreen(
    viewModel: LoginViewModel
) {
    val state by viewModel.uiState.collectAsState()

    Column(modifier = Modifier.padding(16.dp)) {
        OutlinedTextField(
            value = state.email,
            onValueChange = viewModel::onEmailChange,
            label = { Text("Email") }
        )

        OutlinedTextField(
            value = state.password,
            onValueChange = viewModel::onPasswordChange,
            label = { Text("Password") }
        )

        Button(
            onClick = viewModel::onLoginClick,
            enabled = !state.isLoading
        ) {
            Text(if (state.isLoading) "Loading..." else "Login")
        }

        state.errorMessage?.let {
            Text(text = it, color = Color.Red)
        }
    }
}
```

Why this is good:
- single UI state object
- ViewModel owns logic
- Compose only renders state and forwards actions
- no business logic hidden in the UI

### Example 2: Repository Combining Network and Room

```kotlin
data class Article(
    val id: String,
    val title: String,
    val summary: String
)

data class ArticleDto(
    val id: String,
    val title: String,
    val summary: String?
)

@Entity(tableName = "articles")
data class ArticleEntity(
    @PrimaryKey val id: String,
    val title: String,
    val summary: String
)

fun ArticleDto.toEntity(): ArticleEntity {
    return ArticleEntity(
        id = id,
        title = title,
        summary = summary ?: ""
    )
}

fun ArticleEntity.toDomain(): Article {
    return Article(
        id = id,
        title = title,
        summary = summary
    )
}

@Dao
interface ArticleDao {
    @Query("SELECT * FROM articles")
    fun observeAll(): Flow<List<ArticleEntity>>

    @Insert(onConflict = OnConflictStrategy.REPLACE)
    suspend fun insertAll(items: List<ArticleEntity>)
}

interface ArticleApi {
    @GET("articles")
    suspend fun getArticles(): List<ArticleDto>
}

class ArticleRepository(
    private val api: ArticleApi,
    private val dao: ArticleDao
) {
    fun observeArticles(): Flow<List<Article>> {
        return dao.observeAll().map { entities ->
            entities.map { it.toDomain() }
        }
    }

    suspend fun refresh() {
        val remote = api.getArticles()
        dao.insertAll(remote.map { it.toEntity() })
    }
}
```

Why this is good:
- cache is separated from network
- UI can observe local data reactively
- refresh can happen independently
- mappings keep layers clean

### Example 3: WorkManager for Reliable Sync Retry

```kotlin
class UploadPendingNotesWorker(
    appContext: Context,
    params: WorkerParameters,
    private val repository: NotesRepository
) : CoroutineWorker(appContext, params) {

    override suspend fun doWork(): Result {
        return try {
            repository.uploadPendingNotes()
            Result.success()
        } catch (e: IOException) {
            Result.retry()
        } catch (e: Exception) {
            Result.failure()
        }
    }
}
```

Scheduling the worker:

```kotlin
val constraints = Constraints.Builder()
    .setRequiredNetworkType(NetworkType.CONNECTED)
    .build()

val request = OneTimeWorkRequestBuilder<UploadPendingNotesWorker>()
    .setConstraints(constraints)
    .build()

WorkManager.getInstance(context).enqueueUniqueWork(
    "pending_note_upload",
    ExistingWorkPolicy.KEEP,
    request
)
```

Why this is good:
- respects connectivity constraints
- survives app restarts
- retries transient failures
- avoids forcing the UI to stay alive

### Example 4: Sealed UI State for Loading, Success, and Error

```kotlin
sealed interface FeedUiState {
    data object Loading : FeedUiState
    data class Success(val items: List<FeedItem>) : FeedUiState
    data class Error(val message: String) : FeedUiState
}

class FeedViewModel(
    private val repository: FeedRepository
) : ViewModel() {

    private val _uiState = MutableStateFlow<FeedUiState>(FeedUiState.Loading)
    val uiState: StateFlow<FeedUiState> = _uiState

    fun load() {
        viewModelScope.launch {
            _uiState.value = FeedUiState.Loading

            _uiState.value = try {
                val items = repository.loadFeed()
                FeedUiState.Success(items)
            } catch (e: Exception) {
                FeedUiState.Error(e.message ?: "Unable to load feed")
            }
        }
    }
}

@Composable
fun FeedScreen(
    state: FeedUiState,
    onRetry: () -> Unit
) {
    when (state) {
        FeedUiState.Loading -> CircularProgressIndicator()

        is FeedUiState.Success -> LazyColumn {
            items(state.items, key = { it.id }) { item ->
                Text(item.title)
            }
        }

        is FeedUiState.Error -> Column {
            Text(state.message, color = Color.Red)
            Button(onClick = onRetry) {
                Text("Retry")
            }
        }
    }
}
```

Why this is good:
- UI states are explicit and exhaustive
- `when` handles all cases clearly
- easier to test than null-plus-boolean combinations

### Example 5: Search with StateFlow and Debounce

```kotlin
class SearchViewModel(
    private val repository: SearchRepository
) : ViewModel() {

    private val query = MutableStateFlow("")

    val results: StateFlow<List<SearchItem>> = query
        .debounce(300)
        .filter { it.length >= 2 || it.isEmpty() }
        .distinctUntilChanged()
        .flatMapLatest { text ->
            repository.search(text)
        }
        .stateIn(
            scope = viewModelScope,
            started = SharingStarted.WhileSubscribed(5_000),
            initialValue = emptyList()
        )

    fun onQueryChanged(value: String) {
        query.value = value
    }
}
```

Why this is good:
- avoids excessive request spam
- cancels outdated searches
- keeps the UI reactive

---
*This guide is intentionally centered on Kotlin app development, especially Android app development. Use it as a working reference while building screens, shaping architecture, managing lifecycle, handling async work, and shipping production-ready mobile apps.*
