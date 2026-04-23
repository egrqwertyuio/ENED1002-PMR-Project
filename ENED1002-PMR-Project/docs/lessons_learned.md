# Lessons Learned — PMR Team 09.08

## After Subtask 1
Our single color sensor setup was unreliable on dashed lines and inconsistent across lighting conditions. We redesigned with dual color sensors and repositioned sensor placement, which became the foundation for all subsequent development. The takeaway: validate your core sensing hardware under all expected conditions before building on top of it.

## After Status Update 1
Our original timeline was too optimistic given midterm conflicts and the complexity of integrating the pickup mechanism with line following. We learned to build buffer time into the schedule and split into subteams — one focused on line logic, one on the arm — which made our remaining sessions significantly more productive.

## After Status Update 2
Testing subsystems individually is not enough. Line following worked. The pickup mechanism worked. Getting them to work together seamlessly on a live track was an entirely different challenge that we underestimated. Full end-to-end integration testing needs to be prioritized earlier in the process so issues surface before the final stretch.

## General Takeaways

**On mechanical design:** Every mechanical change has downstream consequences. Replacing the rear wheel with a ball bearing solved drag but introduced stability issues that knocked our sensors out of range. Always test the full system after any component change, not just the subsystem you modified.

**On code structure:** Building as a state machine from the beginning made debugging much easier in the final weeks. When something went wrong, we could isolate which state was failing rather than hunting through a single monolithic program.

**On team workflow:** Dividing work by role and strength worked well. The moments where we struggled most were when the mechanical and software sides weren't communicating fast enough about changes — a mechanical mod would break an assumption in the code, or a code change would require a different physical configuration. Earlier and more frequent cross-team check-ins would have saved time.

**On testing:** Paper tracks deform under the robot's weight and wheels, which introduced variability in our test data that wasn't a real reflection of the robot's performance. A more rigid test surface would have given us cleaner data and more confidence going into the final demo.
