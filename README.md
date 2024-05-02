
# PSA.jl

## About

PSA.jl is a partial implementation of [Python for Power System
Analysis (PyPSA)](https://github.com/PyPSA/PyPSA) in
the programming language [Julia](https://julialang.org/).

PSA.jl has been created primarily to take advantage of the speed,
readability and features of the optimisation framework
[JuMP](https://github.com/JuliaOpt/JuMP.jl).

PSA.jl does not yet exist independently of PyPSA, in that you have to
build your network first in PyPSA and export it in CSV format before
PSA.jl can import the network and work with it.

So far the following functionality is implemented in PSA.jl:

* Import from CSV folder
* Export to CSV folder
* Most features of Linear Optimal Power Flow (LOPF), i.e. total electricity/energy system least-cost investment optimisation

What is not yet implemented, but coming soon:

* Normal power flow
* Unit commitment
* Stores
* LOPF with non-unity snapshot weightings
* Links with more than one output
* Standing losses in storage
* Some time-dependent efficiencies and max/min limits
* Import/Export from/to NetCDF


## Required packages

* JuMP.jl
* CSV.jl
* LightGraphs.jl
* DataFrames.jl
* PyCall.jl
* Gurobi.jl

## Basic usage

To install PSA.jl, execute `git clone git@github.com:PyPSA/PSA.jl.git`
in the folder `/path/to/lib` of your choice.

Then in your Julia script:

```julia
push!(LOAD_PATH, "/path/to/lib")

import PSA

network = PSA.import_network("/your/exported/PyPSA/network/")

using Clp

solver = ClpSolver()

m = PSA.lopf(network, solver)

print(m.objVal)
```


## Force specific Conda environment

If you want to use for example the `PyPSA-Eur` environment 
```sh
conda activate pypsa-eur
```

Add the following lines to your code to use the Python binary of same environment
```julia
using PyCall

# Force the specific Python binary; Caveat: overwrites the PyCall build
using Conda
ENV["PYTHON"] = joinpath(Conda.ROOTENV, "envs", ENV["CONDA_DEFAULT_ENV"], "bin", "python")
if PyCall.python != ENV["PYTHON"] 
    @warn "Recompiling with Python binary '$(ENV["PYTHON"])'"
    using Pkg; Pkg.build("PyCall")
end
```


## Licence

Copyright 2017-2018 Fabian Hofmann (FIAS), Tom Brown (KIT IAI)

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation; either [version 3 of the
License](LICENSE.txt), or (at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
[GNU General Public License](LICENSE.txt) for more details.
