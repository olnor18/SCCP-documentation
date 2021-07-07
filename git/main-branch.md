# Main branch requirement for Yggdrasil
The main branch of Yggdrasil should always be kept in a state where it only contains the base services of the platform. This main branch will only contain code that is already acceptance and integration tested, so that any code that is on the main branch is verified to work on the platform, as well as code reviewed. All of the external references on the main branch in Yggdrasil should be made to a main branch in an umbrella chart. If the main branch requirements for umbrella charts that are written below are then fulfilled, we are sure that every service referenced on Yggdrasil is both tested as well as reviewed. 

# Main branch requirement for umbrella charts
All of our umbrella charts should always use the main branch for the newest tested and reviewed code. Making pull requests to the main branch with additional features or changes, ensures that the code has been through a code review, as well as make sure that it is the latest stable and tested version of the umbrella chart. Developing on other branches and referencing the development branch from a dev-cluster is accepted. 