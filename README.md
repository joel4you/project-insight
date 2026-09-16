# project-insight

Then follow the Core Workflow section of the instructions document to create your task, validate it, and submit a pull request. 
Task Proposal

Category: Science Sub-Category: Mechanical Engineering / Structural Mechanics

1. Why is this task genuinely difficult?

The proposed task is a code-based variable-amplitude fatigue assessment of a fillet-welded steel L-bracket subjected to multiaxial operational loading. The agent must determine whether fatigue, yielding, or ultimate failure governs and identify the specific weld detail and load case responsible.

In the real world, this work would be performed by a mechanical design engineer, structural fatigue engineer, or equipment-integrity engineer. Such analysis is valuable for validating brackets and support structures used in industrial equipment, transportation systems, manufacturing machinery, and energy infrastructure.

The task will use synthetic, bespoke engineering data designed to reflect realistic complexity:

A precisely defined steel L-bracket geometry attached to a rigid base plate.

A specified fillet-weld connection with known weld dimensions.

Material properties including elastic modulus, yield strength, and ultimate strength.

Five load cases, each containing 250,000 timestamped rows of six-component force and moment data.

A defined coordinate system, moment reference point, load repetition count, and design life.

Multiple weld details with different orientations and stress responses.

The difficulty is not merely performing a formula calculation. The agent must process a large load history, correctly transform combined forces and moments into stress histories at multiple weld details, perform fatigue cycle counting, and preserve consistency across the entire calculation chain.

Important methodological pitfalls include:

Using the largest instantaneous force as the fatigue-governing condition.

Ignoring moment contributions or applying them about the wrong reference point.

Mixing coordinate frames or using inconsistent force/moment sign conventions.

Applying the wrong weld detail category or S–N curve.

Using an incorrect S–N slope or knee transition.

Applying an unstated mean-stress correction.

Mishandling rainflow residual cycles or cycle ranges.

Treating multiaxial loading as independent scalar loads without following the prescribed combination rule.

Confusing the location of maximum static stress with the location of maximum cumulative fatigue damage.

The dataset will deliberately make the largest peak-force load case different from the fatigue-governing load case, and the fatigue-critical weld detail different from the peak-static-stress detail, ensuring that a simplistic peak-load solution fails a checkable requirement.

2. Intended solution approach

The task will prescribe a single analysis methodology to eliminate ambiguity:

Read the supplied geometry, material, connection, coordinate-frame, and load-history files.

Calculate the required section properties and nominal stress coefficients for each enumerated weld detail.

Transform each six-component load vector into the prescribed normal and shear stress histories at each weld detail.

Apply the specified multiaxial stress-combination rule.

Perform rainflow cycle counting using the prescribed stress-range convention and residue treatment.

Apply the mandated S–N fatigue model, including its fixed detail category, slopes, knee point, and cutoff.

Calculate cumulative fatigue damage using Palmgren–Miner summation.

Separately calculate static yield and ultimate utilisation under the prescribed static load combination.

Identify the governing mechanism, governing load case, and critical weld detail.

Export all required values in the specified machine-readable format.

Fixed methodology

The task statement will explicitly prescribe:

Fatigue standard: EN 1993-1-9 nominal-stress methodology.

Weld detail category: 80.

S–N slopes: m=3 before the knee and m=5 after the knee.

Knee point: 5×10
6
 cycles.

Cutoff: 1×10
8
 cycles.

Mean-stress treatment: No mean-stress correction.

Cycle counting: Defined four-point rainflow implementation, including treatment of residual half cycles.

Stress convention: Prescribed signed nominal normal and shear stress combination.

Static checks: Fixed von Mises yield and ultimate-utilisation equations supplied in the task specification.

This ensures that two competent experts implementing the stated method should converge on the same answer, subject primarily to numerical and implementation differences.

Estimated effort: A qualified fatigue or mechanical engineer would require approximately 8–15 hours, including understanding the geometry and conventions, implementing the load transformation and rainflow analysis, validating intermediate calculations, and producing the required output package.

3. How will the solution be verified?

The agent must produce a machine-checkable solution.json file and specified intermediate result files. The complete schema will be published in the task statement, including field names, units, allowed values, sign conventions, and required precision.

Required outputs

Field

	

Required format

	

Verification




critical_location_id

	

Exact enum from supplied weld-detail list

	

Exact match




governing_load_case_id

	

Exact enum from supplied load-case list

	

Exact match




governing_mechanism

	

fatigue, yielding, or ultimate

	

Exact match




max_principal_stress_MPa

	

Numeric, MPa

	

±2%




max_von_mises_stress_MPa

	

Numeric, MPa

	

±2%




miner_damage_sum

	

Numeric, dimensionless

	

±10%




predicted_fatigue_life_cycles

	

Numeric, cycles

	

±10%




section_modulus_mm3

	

Numeric, mm³

	

±1%




critical_detail_damage_fraction

	

Numeric, dimensionless

	

±10%




load_case_damage_contributions

	

Per-case numeric mapping

	

±10%




cycle_histogram

	

Defined stress-range bins and counts

	

Counts and bin definitions validated

Deterministic verification

Correctness will be evaluated through:

Schema validation: All required fields, units, enums, and data types must be present.

Geometry cross-checks: Section properties and stress coefficients will be compared with independently calculated reference values.

Stress-history checks: Selected stress-history samples at each critical detail will be checked against reference values.

Rainflow checks: Cycle counts and stress-range bins will be compared against a reference implementation using the prescribed algorithm and residue handling.

Fatigue checks: Damage sums and fatigue-life values will be checked against independently generated reference results.

Static checks: Peak stress and utilisation values will be checked against fixed equations and tolerances.

Decision checks: The exact governing mechanism, load case, and critical location must match the reference classification.

The tolerances will be established using at least two independent implementations of the prescribed method, including a reference implementation and a separately developed validation implementation. The ±10% damage tolerance is intended to accommodate documented numerical differences in cycle binning and residue handling—not differences in fatigue standards, S–N curves, or physical assumptions, which are fixed by the task.

A peak-load-only approach, an incorrect S–N curve, or a wrong critical-location selection should fail the exact-match and numerical checks.

4. Category and sub-category justification

Category: Science Sub-Category: Mechanical Engineering / Structural Mechanics

This task belongs in Science because it applies mechanics, materials science, fatigue theory, and numerical analysis to a physical engineering system. The sub-category is Mechanical Engineering / Structural Mechanics because the core work involves load-path analysis, stress transformation, welded-connection behavior, and structural fatigue assessment under multiaxial loading.

The task is appropriate for this category because it requires domain-specific engineering reasoning and computational validation rather than general-purpose arithmetic or text generation
