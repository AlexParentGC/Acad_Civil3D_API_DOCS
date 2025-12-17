# French Translation Task List
**15 Geometry Classes Awaiting Translation**

## Overview
This document tracks the French translation work for 15 newly documented geometry classes. All English documentation is complete (~2,100 lines). French translations are pending.

---

## Translation Tasks

### NURBS Curves (2 files)
- [ ] **NurbCurve3d.md** (240 lines)
  - Source: `AutoCAD/Geometry/NurbCurve3d.md`
  - Target: `fr/AutoCAD/Geometry/NurbCurve3d.md`
  - Priority: CRITICAL
  - Notes: Complex NURBS documentation with 6 examples

- [ ] **NurbCurve2d.md** (90 lines)
  - Source: `AutoCAD/Geometry/NurbCurve2d.md`
  - Target: `fr/AutoCAD/Geometry/NurbCurve2d.md`
  - Priority: CRITICAL

### Elliptical Arcs (2 files)
- [ ] **EllipticalArc3d.md** (180 lines)
  - Source: `AutoCAD/Geometry/EllipticalArc3d.md`
  - Target: `fr/AutoCAD/Geometry/EllipticalArc3d.md`
  - Priority: HIGH
  - Notes: 6 examples including different plane orientations

- [ ] **EllipticalArc2d.md** (80 lines)
  - Source: `AutoCAD/Geometry/EllipticalArc2d.md`
  - Target: `fr/AutoCAD/Geometry/EllipticalArc2d.md`
  - Priority: HIGH

### 3D Primitives (4 files)
- [ ] **Sphere.md** (120 lines)
  - Source: `AutoCAD/Geometry/Sphere.md`
  - Target: `fr/AutoCAD/Geometry/Sphere.md`
  - Priority: HIGH

- [ ] **Cylinder.md** (150 lines)
  - Source: `AutoCAD/Geometry/Cylinder.md`
  - Target: `fr/AutoCAD/Geometry/Cylinder.md`
  - Priority: HIGH

- [ ] **Cone.md** (160 lines)
  - Source: `AutoCAD/Geometry/Cone.md`
  - Target: `fr/AutoCAD/Geometry/Cone.md`
  - Priority: HIGH
  - Notes: Includes frustum calculations

- [ ] **Torus.md** (150 lines)
  - Source: `AutoCAD/Geometry/Torus.md`
  - Target: `fr/AutoCAD/Geometry/Torus.md`
  - Priority: HIGH
  - Notes: Doughnut, apple, lemon types

### Rays (2 files)
- [ ] **Ray3d.md** (100 lines)
  - Source: `AutoCAD/Geometry/Ray3d.md`
  - Target: `fr/AutoCAD/Geometry/Ray3d.md`
  - Priority: HIGH

- [ ] **Ray2d.md** (60 lines)
  - Source: `AutoCAD/Geometry/Ray2d.md`
  - Target: `fr/AutoCAD/Geometry/Ray2d.md`
  - Priority: HIGH

### Splines (4 files)
- [ ] **CubicSplineCurve3d.md** (130 lines)
  - Source: `AutoCAD/Geometry/CubicSplineCurve3d.md`
  - Target: `fr/AutoCAD/Geometry/CubicSplineCurve3d.md`
  - Priority: HIGH

- [ ] **CubicSplineCurve2d.md** (70 lines)
  - Source: `AutoCAD/Geometry/CubicSplineCurve2d.md`
  - Target: `fr/AutoCAD/Geometry/CubicSplineCurve2d.md`
  - Priority: HIGH

- [ ] **Polyline3d.md** (130 lines)
  - Source: `AutoCAD/Geometry/Polyline3d.md`
  - Target: `fr/AutoCAD/Geometry/Polyline3d.md`
  - Priority: HIGH

- [ ] **Polyline2d.md** (70 lines)
  - Source: `AutoCAD/Geometry/Polyline2d.md`
  - Target: `fr/AutoCAD/Geometry/Polyline2d.md`
  - Priority: HIGH

### NURBS Surface (1 file)
- [ ] **NurbSurface.md** (200 lines)
  - Source: `AutoCAD/Geometry/NurbSurface.md`
  - Target: `fr/AutoCAD/Geometry/NurbSurface.md`
  - Priority: HIGH
  - Notes: Complex surface documentation with control point grids

---

## Translation Guidelines

### Technical Terminology
Use consistent French technical terms:
- **NURBS** → NURBS (keep acronym)
- **Control Points** → Points de contrôle
- **Knots** → Nœuds
- **Weights** → Poids
- **Degree** → Degré
- **Sphere** → Sphère
- **Cylinder** → Cylindre
- **Cone** → Cône
- **Torus** → Tore
- **Ray** → Rayon
- **Spline** → Spline
- **Polyline** → Polyligne
- **Surface** → Surface
- **Ellipse** → Ellipse
- **Arc** → Arc

### Code Examples
- Keep all C# code in English (variable names, methods, etc.)
- Translate only comments within code
- Translate output messages (ed.WriteMessage strings)

### Structure
Maintain identical structure to English:
- Same headings
- Same tables
- Same number of examples
- Same cross-references

---

## Progress Tracking

### Statistics
- **Total Files:** 15
- **Total Lines:** ~2,100
- **Completed:** 0/15 (0%)
- **Remaining:** 15/15 (100%)

### By Category
| Category | Files | Lines | Status |
|----------|-------|-------|--------|
| NURBS Curves | 2 | ~330 | ⬜ Pending |
| Elliptical Arcs | 2 | ~260 | ⬜ Pending |
| 3D Primitives | 4 | ~580 | ⬜ Pending |
| Rays | 2 | ~160 | ⬜ Pending |
| Splines | 4 | ~470 | ⬜ Pending |
| NURBS Surface | 1 | ~200 | ⬜ Pending |
| **TOTAL** | **15** | **~2,100** | **⬜ 0%** |

---

## Recommended Translation Order

1. **NurbCurve3d.md** (most critical, most complex)
2. **NurbCurve2d.md** (complete NURBS pair)
3. **EllipticalArc3d.md** (most common after circles)
4. **EllipticalArc2d.md** (complete ellipse pair)
5. **Sphere.md** (simplest 3D primitive)
6. **Cylinder.md**
7. **Cone.md**
8. **Torus.md** (complete 3D primitives)
9. **Ray3d.md**
10. **Ray2d.md** (complete rays)
11. **CubicSplineCurve3d.md**
12. **CubicSplineCurve2d.md**
13. **Polyline3d.md**
14. **Polyline2d.md** (complete splines)
15. **NurbSurface.md** (most complex, save for last)

---

## Verification Checklist

After translation, verify:
- [ ] All 15 French files created in `fr/AutoCAD/Geometry/`
- [ ] File count matches: 31 files in both English and French
- [ ] All technical terms translated consistently
- [ ] All code examples preserved correctly
- [ ] All cross-references updated to point to French files
- [ ] All tables formatted correctly
- [ ] All headings translated
- [ ] README.md updated to reflect French translations

---

## Notes

- **English Documentation:** ✅ Complete (created 2025-12-16)
- **French Translation:** ⬜ Pending
- **Estimated Effort:** 3-5 hours for all 15 files
- **Can be done in batches:** Recommend 5 files per session

---

## Related Documents

- English source files: `AutoCAD/Geometry/*.md`
- Previous French translations: `fr/AutoCAD/Geometry/` (16 existing files)
- Translation quality reference: `fr/AutoCAD/Core/Database.md`
- Undocumented classes analysis: See artifact `undocumented_geometry_analysis.md`

---

**Last Updated:** 2025-12-16  
**Status:** Ready for translation
