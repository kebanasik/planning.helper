 # Planning Helper

## Project name
Planning Helper

## Project description
Planning Helper is a web application for Product Owners to manage team availability and capacity. It allows importing absence schedules from a fixed-format Excel file, managing teams, roles, and members, and visualizing data in a calendar view. The tool helps with realistic sprint and project planning by providing clear, up-to-date information on team capacity.

## Tech stack
- **Frontend:** Astro 5, React 19, TypeScript 5, Tailwind CSS 4, shadcn/ui
- **Backend:** Supabase (PostgreSQL, authentication, BaaS, open source, optional self-hosting)
- **CI/CD:** GitHub Actions
- **Hosting:** DigitalOcean (Docker)
- **Node.js:** 22.14.0

## Getting started locally
1. Ensure you have Node.js version 22.14.0 installed (see `.nvmrc`).
2. Install dependencies:
   ```sh
   npm install
   ```
3. Start the development server:
   ```sh
   npm run dev
   ```
4. Open your browser at the provided local address to view the app.

## Available scripts
- `npm run dev` – Start the development server
- `npm run build` – Build the project for production
- `npm run preview` – Preview the production build
- `npm run lint` – Run ESLint
- `npm run lint:fix` – Fix lint issues automatically
- `npm run format` – Format code with Prettier

## Project scope
- Import absences from a fixed-format Excel file (`.xlsx`)
- Basic validation of imported data (12 months, team members by name)
- Calendar view showing absences for 6 months vertically, sticky name column, color-coded cells
- Team, role, and member management (edit, no member deletion, unique name+surname)
- Logging of import errors and warnings
- Data archiving after import (no UI access)
- No public registration, PO accounts created in the database
- MVP: no HR integration, no editing absences in calendar, no mobile app, no data sharing between teams

## Project status
- Proof of Concept (PoC) / MVP
- Core features under active development

## License
This project is licensed under the MIT License. See the [LICENSE](./LICENSE) file for details.
