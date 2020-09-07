Procedural Physically based BRDF for Real-Time Rendering of Glints - Dictionary generator
=========================================================================================

The implementation of dictionary generator of the paper: Procedural Physically
based BRDF for Real-Time Rendering of Glints. 

Xavier Chermain (ICUBE), Basile Sauvage (ICUBE), Jean-Michel Dishler (ICUBE) and
Carsten Dachsbacher (KIT).

Accepted for [Pacific Graphic
2020](https://pg2020.org/) and for CGF special issue.

The implementation reproduces the dictionary generation algorithm.

GitHub repository: <https://github.com/ASTex-ICube/real_time_glint_dictgenerator>.
See <https://github.com/ASTex-ICube/real_time_glint> to know how to use the dictionary in a renderer.

The generator is part of the [pbrt-v3](https://github.com/mmp/pbrt-v3) tool
`imgtool`, implemented in the function `dictgenerator` in the following file
`src/tools/imgtool.cpp`.

After the build (see [pbrt-v3 building page](https://github.com/mmp/pbrt-v3)),
launch the generator with the command:

`imgtool dictgenerator -outfile dict.exr -nlevels 16 -N 192 -distreso 64 -alpha_dict 0.5 -std_gaussian_lobe 0.02`

to get the dictionary used in the paper. It contains 192 multiscale marginal
distributions with 16 LODs. The size of the tabulated single scale marginal
distribution is 64, the alpha roughness of the target distribution is set to 0.5
and the standard deviation of the gaussian lobe is set to 0.02. This command
generates 1,024 `.exr` files in the runtime directory, each one containing three
single scale marginal distributions. The file naming convention is:

`<outfile parameter>_<triple distribution number>_<LOD number>.exr`

These files will be read by the OpenGL renderer located in the supplementary
material.

The user manual of the command is:
```
dictgenerator options:
    --outfile          Path and base name of the files of the dictionary.
                       Default: dict.exr
    --nlevels          Number of levels of detail. Default: 16
    --N                Number of multiscale marginal distributions. Must be a
                       multiple of 3 (one distribution per RGB color channel).
                       Default: 192
    --alpha_dict       Alpha roughness of the target distribution. Controls the
                       size of the specular lobe at the highest level of detail.
                       Default: 0.5.
    --std_dev_lobe     Standard deviation of the gaussian lobes. Controls the 
                       size of the glint specular lobe. Default: 0.02
    --distreso         Single scale marginal distribution resolution.
                       Default: 128
    --delta            Sampling rate used by numerical integration during 
                       normalisation. Defaut: 0.001
```