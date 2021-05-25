```
DECORATORS
==========

`offset`: Randomly offsets a feature's generation, relative to the original position, based on 3 value providers. One value will be picked for each value provider accepted, and then the feature will be generated with a relative offset equal to the value picked for each direction. This decorator can potentially replace the `square`, `iceberg` and `spread_32_above` decorators.
	Config:
		- `x`: The value by which to offset the feature's generation on the X-axis. Accepts an int value provider. Range is -24 to 24. Required.
		- `y`: The value by which to offset the feature's generation on the Y-axis. Accepts an int value provider. Range is unrestricted. Required.
		- `z`: The value by which to offset the feature's generation on the Y-axis. Accepts an int value provider. Range is -24 to 24. Required.

`repeat`: Works somewhat like `offset`. Essentially, a box is defined using the 6 value providers accepted as inputs to this decorator. Then, at every point within that box, with a probability equal to `chance`, an attempt is made to generate the feature.
	Config:
		- `min_x`: The minimum value for the X-axis range upon which the feature will be repeated. Accepts an int value provider. Range is -16 to 16. Required.
		- `min_y`: The minimum value for the Y-axis range upon which the feature will be repeated. Accepts an int value provider. Range is unrestricted. Required.
		- `min_z`: The minimum value for the Z-axis range upon which the feature will be repeated. Accepts an int value provider. Range is -16 to 16. Required.
		- `max_x`: The maximum value for the X-axis range upon which the feature will be repeated. Accepts an int value provider. Range is -16 to 16. Required.
		- `max_y`: The maximum value for the Y-axis range upon which the feature will be repeated. Accepts an int value provider. Range is unrestricted. Required.
		- `max_z`: The maximum value for the Z-axis range upon which the feature will be repeated. Accepts an int value provider. Range is -16 to 16. Required.
		- `generate_identical_features`: If true, the feature will be cloned (generated exactly the same each time, using the seed from the feature's origin). If false, the feature will generate randomly using a seed based on its own starting position each time.
		- `chance`: The chance to generate the feature, between 0.0 and 1.0. Float. Required.


`conditional`: This is a parent decorator for 'if this, then that'. The feature will only generate if all of the conditions are met.
	Config:

	`conditional_single_block`: This field causes the generation of the feature to succeed or fail (based on whether `inclusive_condition` is true or false, respectively) if a given block tag is present at the requested relative position. Optional field.
		Config:
			- `inclusive_condition`: This value controls whether the condition is inclusive or exclusive. If true, the block tag MUST be present for generation to be attempted. If false, the block tag MUST NOT be present for generation to be attempted. Accepts a boolean value. Required.
			- `block_tag`: The block tag which must be present (or not present) in order to generate the feature. Accepts a block tag. Required.
			- `x_offset`: The relative distance on the X-axis where the required block is expected. Accepts an int value provider. Range is -16 to 16. Optional (defaults to 0).
			- `y_offset`: The relative distance on the Y-axis where the required block is expected. Accepts an int value provider. Range is unrestricted. Optional (defaults to 0).
			- `z_offset`: The relative distance on the Z-axis where the required block is expected. Accepts an int value provider. Range is -16 to 16. Optional (defaults to 0).

	`conditional_nearby_blocks`: This field causes the generation of the feature to succeed or fail (based on whether `inclusive_condition` is true or false, respectively) if the number of blocks belonging to `block_tag`, within the box defined by `x_radius`, `y_radius` and `z_radius`, is between `min_count` and `max_count`. Optional field.
		Config:
			- `inclusive_condition`: This value controls whether the condition is inclusive or exclusive. If true, the number of blocks belonging to block tag MUST be between `min_count` and `max_count` for generation to be attempted. If false, the number of blocks belonging to block tag MUST be between `min_count` and `max_count` for generation to be attempted. Accepts a boolean value. Required.
			- `block_tag`: The block tag which must be present (or not present) in order to generate the feature. Accepts a block tag. Required.
			- `x_radius`: The X-axis radius of the box in which to check for `block_tag`. Accepts an int value provider. Range is 0 to 16. Required.
			- `y_radius`: The Y-axis radius of the box in which to check for `block_tag`. Accepts an int value provider. Range is 0 to 2048. Required.
			- `z_radius`: The Z-axis radius of the box in which to check for `block_tag`. Accepts an int value provider. Range is 0 to 16. Required.
			- `min_count`: The minimum number of blocks. Int. Required.
			- `max_count`: The maximum number of blocks. Int. Required.

	`conditional_biome`: This field causes the generation of the feature to succeeed or fail (based on whether `inclusive_condition` is true or false, respectively) if it is within a particular array of biomes. This is similar to the function of the `freeze_top_layer` feature on biome borders.
		Config:
			- `inclusive_condition`: This value controls whether the condition is inclusive or exclusive. If true, the feature MUST be within the biome to generate. If false, the feature MUST NOT be within the biome to generate.
			- `biomes`: Accepts an array of strings corresponding to biomes. Nonexistent biomes will be ignored. Required field.

	`conditional_water_depth`: This field causes the generation of the feature to fail if the number of water blocks above the target block is not between `min_depth` and `max_depth`.
		Config:
			- `min_depth`: Int. Optional - Defaults to 0
			- `max_depth`: Int. Required.






FEATURES
========

`feature_array`: Generates the features listed in order, starting at the same position as instructed by previous decorators.
	Config:
		- `features`: Accepts an array of features. This is the same syntax as the `simple_random_selector` decorator.

`simple_structure`: Generates a single structure. This structure is picked randomly from the array and run through the requested processors.
	Config:
		- `structures`: Accepts an array of objects.
			Object config:
				`structure`: A string pointing to the structure. Required
				`processor`: Processor to apply to the structure. Optional, defaults to `minecraft:empty`
				`weight`: The weight of the structure in the random calculation. Optional, defaults to 1
				`cannot_replace`: Accepts a block tag, blocks in this tag will not be replaced by the structure

```
