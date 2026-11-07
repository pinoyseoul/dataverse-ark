# Contributing to Dataverse ARK SPI Provider

Thank you for your interest in contributing to the Dataverse ARK SPI Provider! We welcome community contributions, bug reports, and enhancements from the Harvard Dataverse and research data management community.

## Development Workflow

1. Fork the repository and create your feature branch:
   ```bash
   git checkout -b feature/my-new-feature
   ```

2. Make your modifications, ensuring that:
   * Java code adheres to standard Java 17 conventions.
   * New functionality maintains compatibility with Harvard Dataverse 6.x SPI interfaces.
   * All settings remain configurable via standard environment variables and JVM system properties.

3. Build and test locally:
   ```bash
   mvn clean package
   ```

4. Commit your changes with clear, descriptive commit messages:
   ```bash
   git commit -am 'feat: add support for custom resolver metadata prefixes'
   ```

5. Push to your branch and open a Pull Request.

## Code of Conduct

Please help us keep this project a welcoming and respectful space for academic collaborators, librarians, and developers worldwide.
