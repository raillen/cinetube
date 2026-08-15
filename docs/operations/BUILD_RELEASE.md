# Build and Release

## Desktop

Releases devem pin Wails v3 e toolchains, gerar hashes/artifacts e registrar OS/arch. Nunca depender de `latest` no build de release.

## Gates

1. clean build;
2. unit/integration/E2E críticos;
3. migration/backup restore test;
4. dependency/security scan;
5. accessibility smoke;
6. performance comparison;
7. provider contract smoke;
8. docs delta;
9. release verifier.

## Platform matrix

Windows/Linux/macOS entram somente após pipeline e hardware/VM smoke adequados. Não declarar suporte onde não houve teste.
