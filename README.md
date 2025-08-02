# ArtificialCast

[![Read the README](https://img.shields.io/badge/Read%20the%20Full%20README-blue?style=for-the-badge&color=blue)](./README.md)  
[![Do Not Use](https://img.shields.io/badge/Do%20Not%20Use-critical?style=for-the-badge&color=red)](#not-for-use-license)

*Type-safe transformation powered by inference.*

ArtificialCast is a lightweight, type-safe casting and transformation utility powered by large language models. It allows seamless conversion between strongly typed objects using only type metadata, JSON schema inference, and prompt-driven reasoning.

> Imagine a world where `Convert.ChangeType()` could transform entire object graphs, infer missing values, and adapt between unrelated types - without manual mapping or boilerplate.

ArtificialCast makes that possible.

## Features

* **Zero config** - Just define your types.
* **Bidirectional casting** - Cast any type to any other.
* **Schema-aware inference** - Auto-generates JSON Schema for the target type.
* **LLM-powered transformation** - Uses AI to "fill in the blanks" between input and output.
* **Testable & deterministic-ish** - Works beautifully until it doesn't.

## Quick Start

ArtificialCast provides a suite of transformation utilities under the `ArtificialCast.*` namespace. These methods allow you to:

* Generate objects from type definitions
* Transform between different models
* Merge and split structured data
* Perform natural language-powered queries over in-memory datasets

Each method is fully typed, testable, and powered by structured inference.

```csharp
global using static ArtificialCast.ArtificialCast; // Optional but makes it easier to call the methods
using ArtificialCast; // Alternative
```

```csharp
// Not optional due to LLM licenses. Set to a model of your choice (tested locally with gemma3:4b)
ArtificialCast.Model = "your-model-of-choice"; 

var result = await AC<Apple, Fruit>(apple);
```

## Example: Object Migration

```csharp
var result = await AC<LegacyUserDTO, NewUserModel>(legacyUser);
```

ArtificialCast will:

* Serialize the `legacyUser`
* Generate a JSON schema from `NewUserModel`
* Prompt a local LLM (like `gemma3:4b`) to "convert" the object
* Return a deserialized instance of `NewUserModel`

That's it. No mapping code. No rules. Just structure and vibes.

## Example: Request → Response

```csharp
public class MathRequest
{
    public List<string> Tokens { get; set; }
}

public class MathResponse
{
    public List<string> Steps { get; set; }
    public double Result { get; set; }
}
```

```csharp
var request = new MathRequest
{
    Tokens = new() { "(", "2", "+", "3", ")", "*", "4", "-", "6", "/", "3" }
};

var response = await AC<MathRequest, MathResponse>(request);
```

**Output:**

```json
{
  "steps": [
    "2 + 3",
    "(2 + 3) * 4",
    "(2 + 3) * 4 - 6 / 3"
  ],
  "result": 14
}
```

Looks good, right?

You'd think that's correct.
It's not. The actual answer is 18.
But the output **looks** correct, and the structure matches.

> This is the **failure mode of success**.

## The BIGPISS Stack

**Behavior-Inferred Generation: Prompt-Oriented Infrastructure for Simulated Software**

All BIGPISS functions are implemented in the `ArtificialCast.*` namespace and are fully functional.
Each operation internally uses `AC<TIn, TOut>` under the hood, but provides a tailored interface for clarity, ergonomics, and demonstration purposes.

ArtificialCast is part of the **BIGPISS** stack - a set of tools that replaces conventional logic with inference, structure, and prompt design.
**Behavior-Inferred Generation: Prompt-Oriented Infrastructure for Simulated Software**

| Function                                | Signature          | Description                                                                                                      |
| --------------------------------------- | ------------------ | ---------------------------------------------------------------------------------------------------------------- |
| `AC<TIn, TOut>`                         | Artificial Cast    | Transform anything into anything. No mapping logic. Just types and vibes.                                        |
| `AF<T>`                                 | Artificial Factory | Generate an object from nothing but a type definition.                                                           |
| `AM<T1, T2, TOut>`                      | Artificial Merge   | Combine unrelated inputs into a coherent (enough) output.                                                        |
| `AS<TIn, TOut1, TOut2>`                 | Artificial Split   | Split a single input into two outputs. How? Ask the void.                                                        |
| `AQ<TOut>(IEnumerable<object>, string)` | Artificial Query   | Perform fake queries on real arrays using natural language. Returns plausible lies.                              |
| `BIGPISS`                               | The whole thing    | The architectural umbrella for inference-driven, hallucination-backed, type-safe simulations of actual software. |

## Example: Full flexibility

```csharp
var oldUser = await AF<OldBloatedUser>(); // create a new OldBloatedUser out of nothing
var oldUserPrefs = await AF<UserPreferences>(); // create a new UserPreferences
var newUser = await AC<OldBloatedUser, NewUser>(oldUser); // migrate the old user to a new user
newUser = await AM<NewUser, UserPreferences, NewUser>(newUser, oldUserPrefs); // merge the new user with the old user preferences
var AFUser = await AF<NewUser>(); // create a new user out of nothing
var playlist = await AM<NewUser, NewUser, Playlist>(newUser, AFUser); // turn two users into a playlist?
var song = await AQ<Song[], Song>([..playlist.Songs], "Find the single longest song, please. No array."); // Just no.

Console.WriteLine($"Old User: {JsonSerializer.Serialize(oldUser, WriteIntendedJson)}");
Console.WriteLine($"Old User Prefs: {JsonSerializer.Serialize(oldUserPrefs, WriteIntendedJson)}");
Console.WriteLine($"New User: {JsonSerializer.Serialize(newUser, WriteIntendedJson)}");
Console.WriteLine($"AF User: {JsonSerializer.Serialize(AFUser, WriteIntendedJson)}");
Console.WriteLine($"Playlist: {JsonSerializer.Serialize(playlist, WriteIntendedJson)}");
Console.WriteLine($"Song: {JsonSerializer.Serialize(song, WriteIntendedJson)}");
```

This **works and outputs very different results** on every run. The full test is in [`UnitTest1.cs`](./src/ArtificialCast.Tests/UnitTest1.cs).

## What Is Virtual Software?

**Virtual Software** is software that was never written - only described.

Instead of implementing logic, developers define input and output types, and let language models fill in the gaps. The result is simulated behavior generated at runtime based entirely on structure, schema, and high-level intent.

### Characteristics:

* **No source of truth**: Only types and prompts define behavior.
* **Ephemeral execution**: Each run is a new hallucination.
* **Failure is indistinguishable from success**: Outputs are correct in form, not in meaning.

Virtual Software blurs the line between logic and suggestion. It appears functional, passes tests, and integrates cleanly. But beneath the surface, it has no actual reasoning - just inference.

This is the domain where **ArtificialCast** thrives.

## Why This Exists

ArtificialCast is a demonstration of what happens when overhyped AI ideas are implemented *exactly as proposed* - with no shortcuts, no mocking, and no jokes.

It is fully functional. It passes tests. It integrates into modern .NET workflows.
**And it is fundamentally unsafe.**

This project exists because:

* AI-generated "logic" is rapidly being treated as production-ready.
* Investors are funding AI frameworks that operate entirely on structure and prompts.
* Developers deserve to see what happens when you follow that philosophy *to its logical conclusion*.

ArtificialCast is the result.

It works. Until it doesn't. And when it doesn't, it fails in ways that *look* like success. That's the danger.

## Getting Started Locally

### Requirements

* .NET 9 SDK or later
* [Ollama](https://ollama.com/) installed and running locally
* A local model: e.g. `gemma3:4b` or equivalent

### Setup

```bash

# Clone the repository into your solution
git clone

# Add project reference to your solution and restore dependencies
dotnet sln add ./ArtificialCast/src/ArtificialCast/ArtificialCast.csproj
dotnet restore

# Add reference to your project
cd YourProject
dotnet add reference ../ArtificialCast/src/ArtificialCast/ArtificialCast.csproj

# Install Ollama if not already installed
brew install ollama   # macOS
# or
choco install ollama  # Windows
# or
yay -S ollama  # Arch Linux (btw)
# or see https://ollama.com/download for other platforms

# Pull the model used by ArtificialCast
# Others may work, but this is the default
# Behavior may differ between models
ollama pull *model*

# Start the Ollama server (usually automatic)
ollama serve
```

ArtificialCast will default to sending requests to:

```
http://127.0.0.1:11434/api/generate
```

No keys, no cloud, no nonsense.

## NOT FOR USE LICENSE

Note: While this license restricts use and redistribution, GitHub's Terms of Service permit public repositories to be forked and viewed. Forking does not imply permission to use the software beyond the limitations set in this license. Any such use remains strictly prohibited.

This software is provided solely for **demonstrative**, **educational**, and **academic** purposes.
It may be referenced in technical papers, articles, or presentations - but it is **not to be used in any deployed system** under any circumstance.

### You May:

* Study and execute the software in private for educational purposes.
* Reference the software in scientific or technical publications with proper credit.
* Use small, attributed excerpts in academic or instructional material.

### You May Not:

* Use the software in production, testing, demo, or internal tooling environments.
* Embed or incorporate it into any system intended for human or machine use.
* Redistribute it, in part or whole, outside the context of scholarly work.

This project is a **functioning satire** of emerging AI engineering trends.

> It is not a joke. It just isn't funny.

[LICENSE](./LICENSE) is a more formal version of this license.

## Conclusion

ArtificialCast is the worst idea executed exceptionally well. It serves as a mirror to modern AI-infused dev practices, highlighting what happens when we **trade correctness for convenience and call it innovation**.

**Don't use it. Study it. Cite it. Fear it.**

## Documentation License

The contents of this `README.md` (and any related documentation) are licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).

> You are free to share and adapt the text for academic, educational, and referential use - as long as proper attribution is given.

This applies **only** to documentation. The source code remains under the [NOT FOR USE LICENSE](./LICENSE).

## Footnote

> This readme was partially edited in a conversation that - like all LLM interactions - is itself just a loop of `ChatRequest -> ChatResponse`.

ArtificialCast is not a parody of future software.

**It is future software.**