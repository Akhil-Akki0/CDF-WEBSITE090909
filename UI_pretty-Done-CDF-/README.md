# CDF Client Website — OpenFOAM CFD Platform

A React/TypeScript and Node.js/Express workbench for preparing and executing native OpenFOAM cases.

## Accuracy contract

- The backend runs native `blockMesh`, `snappyHexMesh`, `checkMesh`, `simpleFoam`, and `foamToVTK` only.
- A validated STL payload is required. No placeholder geometry or synthetic mesh is created.
- If OpenFOAM is unavailable, `/readyz` returns `503` and the simulation fails safely.
- CFD metrics are returned only after native meshing, solver execution, VTK extraction, and force-coefficient parsing all succeed.
- Failed or incomplete runs return `metrics: null`; the UI shows unavailable values instead of estimates.
- The frontend uses local/system font stacks and does not load remote Google Fonts.

## Prerequisites

- Node.js 18+
- npm
- Native OpenFOAM with these commands available on `PATH`:
  `blockMesh`, `snappyHexMesh`, `checkMesh`, `simpleFoam`, and `foamToVTK`

## Quickstart

```bash
npm install
npm run lint
npm run build
npm run dev
```

Open http://localhost:3000.

Check backend readiness:

```bash
curl -i http://localhost:3000/readyz
curl http://localhost:3000/api/health
```

A `503` readiness response means the host is not ready for accurate CFD execution. Install/source OpenFOAM and restart the backend; do not interpret the setup preview or unavailable fields as solver output.

## Production deployment

The frontend may be hosted separately, but the backend must run on a persistent Linux/WSL2/Docker host where OpenFOAM is installed and sourced. See `VERCEL_DEPLOYMENT.md` for the separation between frontend hosting and the native solver backend.
