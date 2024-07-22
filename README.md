# Guide

Guide for viewers, explaining the various features available on our extension in multiple languages

## How to run it

1. Clone the repository
2. Open the repository and run `pnpm i` to install the dependencies
3. Run `pnpm dev` to start the development server
4. Open `http://localhost:5173` in your browser

## Contribution Guide for Translations

Thank you for your interest in contributing to our guide translations! Your help is invaluable in making this extension accessible to a global audience. Here's how you can contribute:

### Langs Structure

All translations are stored in the `src/assets/langs/` directory. Each language has its own JSON file. For example:

- English: `src/assets/langs/en.json`
- French: `src/assets/langs/fr.json`
- Spanish: `src/assets/langs/es.json`

### How to Contribute

1. **Add or Modify Language Files**:
    - Add or modify language files in the `src/assets/langs/` directory. You'll find various files already named with ISO 639-1 language codes.

    **For Adding a New Language**:
    - Ensure the file contains the same number of lines as other files to verify that nothing has been missed.
    - Add your language's name in your language to the `src/assets/langs/available_langs.json` file. For example, for French: `"fr": "Français"`.

2. **Submit a Pull Request**:
    - Create a branch for your changes.
    - Once your changes are made, submit a pull request with a description of the modifications made or the new language added.

    For an example of a typical pull request, you can refer to [this PR](https://github.com/UltimateCC/guide/commit/86e66224a0f0551c0db2d513e79cde48782cb665).

### Best Practices

- **Consistency**: Ensure your translation aligns with the tone and style of other languages.
- **Accuracy**: Verify that your translation is correct and free of errors.
