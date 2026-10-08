# 📚 StudyFlow

Aplicativo Flutter para organização de estudos por matérias e prática com questões reais do ENEM.

O **StudyFlow** permite navegar por áreas de estudo, filtrar conteúdos por nível de dificuldade e resolver questões do ENEM consumidas de uma API pública, com correção instantânea do gabarito.

## ✨ Funcionalidades

- **Áreas de estudo:** Matemática, Linguagens, Ciências Humanas e Ciências da Natureza
- **Conteúdos por nível:** filtro por `Básico`, `Intermediário` e `Avançado` com `ChoiceChip`
- **Detalhe do conteúdo:** título, nível e descrição
- **Questões do ENEM:**
  - Lista de questões via API `api.enem.dev`
  - Tela de detalhe com enunciado, imagens, introdução e alternativas
  - Seleção interativa de alternativa com feedback de acerto/erro
  - Tratamento de loading, erro com botão `Tentar novamente` e lista vazia
- **UI em Material 3** com `ColorScheme.fromSeed`

## 🛠️ Tecnologias

- **Flutter** (Dart SDK `^3.12.2`)
- **Material 3**
- Pacotes:
  - `http: ^1.6.0` — consumo da API do ENEM
  - `cupertino_icons: ^1.0.8`
  - `flutter_lints: ^6.0.0` — padrão de lint
  - `flutter_test` — teste de widget

## 📁 Estrutura do projeto

```
lib/
├── main.dart                  # Entry point + StudyFlowApp (MaterialApp)
├── models/
│   ├── materia.dart             # Materia { nome, descricao, conteudos }
│   ├── conteudo.dart            # Conteudo { titulo, descricao, nivel }
│   └── questao_enem.dart        # QuestaoEnem + AlternativaEnem (fromJson)
├── data/
│   └── dados_estudo.dart        # Dados mockados das 4 matérias
├── services/
│   └── enem_service.dart        # GET https://api.enem.dev/v1/exams/{ano}/questions
├── screens/
│   ├── home_page.dart           # Lista de matérias + botão Questões do ENEM
│   ├── materia_page.dart        # Detalhe da matéria + filtro por nível
│   ├── conteudo_page.dart       # Detalhe do conteúdo
│   ├── enem_page.dart           # Lista de questões (FutureBuilder)
│   └── questao_enem_page.dart   # Resolver questão + ver gabarito
└── widgets/
    ├── materia_card.dart
    └── conteudo_card.dart
```

## 🔌 API do ENEM

Serviço em `lib/services/enem_service.dart`:

```dart
GET https://api.enem.dev/v1/exams/{ano}/questions?limit=10&offset=0
```

Comportamento padrão: `buscarQuestoes({ano = 2022})` com timeout de 15s.

Resposta esperada:
```json
{
  "questions": [
    {
      "index": 1,
      "title": "Questão sem título",
      "discipline": "mathematics",
      "context": "...",
      "alternativasIntroduction": "...",
      "correctAlternative": "A",
      "files": ["https://..."],
      "alternatives": [
        { "letter": "A", "text": "...", "file": null }
      ]
    }
  ]
}
```

> Obs: a `EnemPage` exibe o título fixo "Questões do ENEM 2020", mas o service busca por padrão o ano 2022. Ajuste o parâmetro `ano` para alinhar.

## 🚀 Como rodar

Pré-requisitos:
- Flutter SDK instalado (>= 3.12)
- Emulador Android / iOS, Chrome (web) ou dispositivo físico

```bash
# 1. Clonar o repositório
git clone <url-do-repo>
cd study_flow

# 2. Instalar dependências
flutter pub get

# 3. Analisar o código
flutter analyze

# 4. Rodar o app
flutter run

# 5. Rodar os testes
flutter test
```

Dispositivos específicos:
```bash
flutter run -d chrome
flutter run -d windows
flutter run -d android
```

## 🧭 Fluxo de navegação

```
HomePage (Áreas de estudo)
├── MateriaPage (materia selecionada + filtro de nível)
│   └── ConteudoPage (detalhe do conteúdo)
└── EnemPage (lista de questões da API)
    └── QuestaoEnemPage (responder + gabarito)
```

## 🧪 Teste

Teste de widget em `test/widget_test.dart` verifica que o app renderiza o texto `StudyFlow`:

```bash
flutter test
```

## 📌 Melhorias futuras

- [ ] Paginação / filtro por ano e disciplina nas questões do ENEM
- [ ] Busca de conteúdos e matérias
- [ ] Marcar conteúdos como concluídos (persistência local)
- [ ] Histórico de desempenho nas questões
- [ ] Modo offline / cache das questões
- [ ] Sincronizar título da `EnemPage` com o `ano` do `EnemService`
- [ ] Internacionalização e acessibilidade

## 📄 Licença

Projeto privado (`publish_to: none`). Uso livre para fins de estudo.
