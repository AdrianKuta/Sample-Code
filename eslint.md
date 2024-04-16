# eslint

```json
"rules": {
    "no-nested-ternary": "error",
    "promise/prefer-await-to-then": "warn",
    "mozart/no-date-fns-format": "error",
    "mozart/avoid-literals-messages": "error",
    "mozart/await-try-catch": "off",
    "mozart/prefer-optional-syntax": "error",
    "mozart/check-project-folder-structure": "error",
    "mozart/check-translation-keys": "error",
    "mozart/avoid-literal-props": ["error", []],
    "no-unused-vars": "off",
    "i18next/no-literal-string": "error",
    "@typescript-eslint/no-explicit-any": "warn",
    "@typescript-eslint/strict-boolean-expressions": [
      "warn",
      {
        "allowNullableString": true,
        "allowNullableBoolean": true,
        "allowNullableObject": true
      }
    ],
    "@typescript-eslint/no-unused-vars": "error",
    "react/prop-types": 0,
    "import/no-default-export": "error",
    "@typescript-eslint/ban-ts-comment": "off",
    "react/function-component-definition": [
      2,
      {
        "namedComponents": "function-declaration",
        "unnamedComponents": "function-expression"
      }
    ],
    "@next/next/no-html-link-for-pages": "off",
    "react/jsx-key": "error",
    "tailwindcss/no-custom-classname": "off",
    "tailwindcss/no-arbitrary-value": "error",
    "import/no-relative-parent-imports": [
      "off",
      {
        "ignore": [
          "@/ui",
          "@/lib",
          "@/config",
          "@/styles",
          "@/types",
          "@/components",
          "@/api"
        ]
      }
    ],
    "react/react-in-jsx-scope": "off",
    "react/jsx-uses-react": "off",
    "react/self-closing-comp": [
      "error",
      {
        "component": true,
        "html": true
      }
    ],
    "@typescript-eslint/consistent-type-imports": [
      "error",
      {
        "prefer": "type-imports"
      }
    ],
    "no-restricted-syntax": [
      "error",
      {
        "selector": "CallExpression[callee.object.name='console'][callee.property.name=/^(log|error|warn|info|debug)$/]",
        "message": "Using console.log is not allowed. Please use logger instead (src/lib/logger.ts)"
      }
    ],
    "no-restricted-imports": [
      "error",
      {
        "paths": [
          {
            "name": "next/link",
            "message": "Please use next-intl/link instead."
          }
        ]
      }
    ],
    "no-return-await": "warn",
    "filename-rules/match": [2, "kebab-case"]
  },
```