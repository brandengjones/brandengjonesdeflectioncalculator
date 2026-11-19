
# Beam Designer

## Purpose
Create a focused single-file web app that calculates reactions, shear,
moment, and deflection for statically determinate beams. It will also
check bending, shear, and deflection for a selected steel or lumber
member.

## Inputs
- Beam type: simply supported or cantilever.
- Span in feet.
- A load table for point loads and uniformly distributed loads.
- Member type: steel W-shape or lumber.
- For steel: W-shape designation from an embedded library.
- For lumber: species, grade, and member size.
- Optional custom member properties for both beam types.

## Calculations
- Reactions, shear, moment, slope, and deflection from elastic beam theory.
- Use consistent sign conventions and report values for positive and negative
  shear and moment.
- Use US customary units throughout: feet for geometry, kips or pounds for
  load, and ksi or psi for stress.
- Exclude beam self-weight unless the user explicitly adds it as a distributed
  load.
- Include the virtual or superposition-based deflection caused only by the
  loads entered by the user.
- Check bending stress with `fb = M / Sx`.
- Check deflection against a selected or user-entered span/deflection ratio.
  The default is L/360.
- Use ASD for allowable stress checks.
- Steel and lumber allowable stress values will come from the selected
  standard or an embedded test dataset, and the property source or edition
  will be displayed.

## Load inputs
Display loads in an editable table with:

- Load type: point load or uniformly distributed load.
- Magnitude.
- Exact position from the left support for a point load.
- Start and end position for a uniform load.
- Units.
- Remove, duplicate, and edit actions.

Keep an example beam loaded when the app opens.

## Beam properties
### Steel W-shapes
An initial steel catalog will include, at minimum:

- W12x26: `Ix = 204 in^4`, `Sx = 33.4 in^3`, `A = 7.65 in^2`,
  depth approximately 12.2 in, `E = 29,000 ksi`.
- W10x12: `Ix = 53.8 in^4`, `Sx = 12.6 in^3`, `A = 3.54 in^2`,
  depth approximately 9.87 in, `E = 29,000 ksi`.
- W16x26: `Ix = 301 in^4`, `Sx = 38.4 in^3`, `A = 7.68 in^2`,
  depth approximately 15.7 in, `E = 29,000 ksi`.

Store enough cross-section properties for strong-axis bending and shear:
designation, area, depth, flange width and thickness, web thickness,
moment of inertia `Ix`, elastic section modulus `Sx`, and yield strength
`Fy`. Property values will identify their AISC data source and edition.

### Lumber
Allow species and grade selection followed by common member sizes. Store:

- Species and grade.
- Nominal size and actual dimensions.
- Elastic modulus `E`.
- Allowable bending stress `Fb`.
- Allowable shear stress `Fv`.
- Allowable compression perpendicular-to-grain stress `Fc-perp`.
- Moment of inertia `Ix` and section modulus `Sx`.
- Load-duration and wet-service adjustment factors where available.

The app will label NDS edition and adjustment assumptions and not imply
that omitted factors are approved design values.

### Custom properties
For advanced use, allow a custom member entry for:

- `E`, `Ix`, `Sx`, and cross-sectional area.
- `Fb`, `Fy`, or the applicable allowable bending stress.
- `Fv`.
- `Fc-perp`, if lumber support bearing is checked.

## Strength and serviceability checks
### Bending
For steel and lumber, calculate maximum strong-axis bending stress and
compare it with the allowable bending stress.

### Shear
Calculate maximum absolute shear and report average shear where it is
applicable:

- Steel: `fv = V / (tw * d)`.
- Lumber and rectangular members: use the NDS-style reduced shear value
  with clear notation and a shape factor of 1.5 for an initial check.

Compare the result with the selected member's allowable shear stress.

### Deflection
Report maximum absolute deflection and the governing span/deflection ratio,
defaulting to L/360. The result must pass only if it is less than or equal
to the selected limit.

### Lumber bearing
If support or load-point bearing is implemented, check compression
perpendicular to grain using bearing area and `Fc-perp`. Otherwise, list
this check as not applicable or not included.

### Out of scope
The first version does not calculate lateral-torsional buckling, axial
loads, torsion, continuous beams, or indeterminate structures. Steel
members are assumed to be laterally braced when the bending check is used.
This limitation must be visible beside bending results.

## Outputs
- A labeled beam sketch with support locations, span, and loads.
- Reaction forces.
- Shear diagram.
- Moment diagram.
- Deflection diagram.
- Maximum values for shear, moment, and deflection.
- Separate pass/fail indicators for bending, shear, deflection, and
  applicable bearing.
- An assumptions and design limitations panel.
- A summary of member properties and allowable values.

## Look and feel
Clean and professional, accessible, responsive, and suitable for classroom
or engineering-office use. Retain the example beam loaded on startup.
Label units next to every input and result.

## Validation
Include repeatable test cases with the source or hand-calculation method:

1. Simply supported W12x26, span 20 ft, uniform load 1 kip/ft:
   - Beam self-weight excluded.
   - Reactions: 10.0 kips each.
   - Maximum moment: 50.0 kip-ft.
   - Maximum bending stress: approximately 17.96 ksi.
   - Maximum deflection: approximately 0.609 in.
   - Bending stress limit: from the selected ASD allowance.
   - Deflection limit: 240/360 = 0.667 in.
2. Simply supported W12x26, span 20 ft, center point load 10 kips:
   - Maximum moment: 50.0 kip-ft.
   - Maximum deflection: approximately 0.485 in.
3. Cantilever W12x26, span 10 ft, uniform load 1 kip/ft:
   - Fixed-end reaction: 10.0 kips.
   - Fixed-end moment: 50.0 kip-ft.
   - Maximum deflection: approximately 0.366 in.
4. Simply supported beam with one point load and one uniform load:
   - Compare reactions, shear, moment, and deflection against independent
     hand calculations or superposition results.
5. Negative validation: a span outside the supported range or a load outside
   the beam is rejected with a clear input message.

## Error handling
- Reject nonnumeric, negative-span, and out-of-range load positions.
- Prevent missing member properties or unsupported grades from being used.
- Keep invalid entries visible and explain how to correct them.
- Identify whether a result is a calculated value, an assumed value, or a
  catalog value.

## Documentation
- Include a visible methodology summary and property source.
- Separate approved design standards from educational assumptions.
- State that the app is a calculation aid and is not a substitute for
  professional engineering judgment.