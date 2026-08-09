# rebelstack-template

rebelstack-template is a GitHub repository template for quickly bootstrapping new projects with the RebelStack — an opinionated combination of Spring Boot, j2html, and deploy4j that gets you to a running, deployable web application with minimal ceremony.

> Clone this template to start your next project with a consistent structure, sensible defaults, and a ready-to-go deployment pipeline.

## Features

- **Spring Boot** — Production-grade Java web application foundation
- **j2html** — Type-safe, fluent HTML generation in pure Java — no template files required
- **deploy4j** — Lightweight, zero-infrastructure deployment tooling for Java apps
- **Template-Ready** — Designed as a GitHub template repository; every project starts from the same clean baseline
- **Minimal Boilerplate** — Get from `git clone` to a running app in minutes

## Technology Stack

| Layer | Technology |
|---|---|
| Language | Java |
| Framework | Spring Boot |
| View / HTML | j2html |
| Deployment | deploy4j |

## Project Structure

```
rebelstack-template/
├── src/
│   ├── main/
│   │   ├── java/       # Application source code
│   │   └── resources/  # Configuration and static assets
│   └── test/
│       └── java/       # Test source code
└── README.md
```

## Getting Started

### Prerequisites

- Java 21+
- Maven or Gradle (depending on the build file included in the template)

### Create a new project from this template

Click **Use this template** on GitHub, or clone directly:

```bash
git clone https://github.com/teggr/rebelstack-template.git my-project
cd my-project
```

### Run

```bash
./mvnw spring-boot:run
```

Then open [http://localhost:8080](http://localhost:8080) in your browser.

### Build

```bash
./mvnw package
```

## Contributing

Contributions, ideas, and improvements are welcome. Open an issue or submit a pull request.

## License

See [LICENSE](LICENSE) for details.
