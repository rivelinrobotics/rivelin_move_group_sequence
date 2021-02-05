Move Group Sequence
===================

Plan and execute sequences of LIN, PTP and CIRC motions on industrial manipulators.

Based on `pilz_robot_programming <https://github.com/PilzDE/pilz_industrial_motion/tree/melodic-devel/pilz_robot_programming>`_ but adapted to work with any manipulator with a moveit configuration. This requires the `Pilz Industrial Motion Planner <https://ros-planning.github.io/moveit_tutorials/doc/pilz_industrial_motion_planner/pilz_industrial_motion_planner.html>`_ to be installed.

Import
^^^


.. code-block:: python

    from move_group_sequence import MoveGroupSequence, Ptp, Lin, Circ
    from geometry_msgs.msg import Point, Pose
    from moveit_commander import MoveGroupCommander

    PLANNING_GROUP = "manipulator"
    move_group = MoveGroupCommander(PLANNING_GROUP)
    sequencer = MoveGroupSequence(move_group)


Plan and execute
^^^^
.. code-block:: python

    sequencer.plan(Ptp(goal=[0, 0.5, 0.5, 0, 0, 0]))
    sequencer.execute()

.. code-block:: python

    sequencer.plan(Lin(goal=Pose(position=Point(0.2, 0, 0.8))))
    sequencer.execute()

.. code-block:: python

    sequencer.plan(Ptp(goal=Pose(position=Point(0.2, 0, 0.8))))
    sequencer.execute()

.. code-block:: python

    sequencer.plan(Circ(goal=Pose(position=Point(0.2, 0.2, 0.8)), center=Point(0.3, 0.1, 0.8)))
    sequencer.execute()


Relative commands
^^^^^^^^^^^^^^^^^
.. code-block:: python

    sequencer.plan(Ptp(goal=[0.1, 0, 0, 0, 0, 0], relative=True))

.. code-block:: python

    sequencer.plan(Lin(goal=Pose(position=Point(0, -0.2, -0.2)), relative=True))

:py:class:`.Ptp` and :py:class:`.Lin` commands can also be stated as relative commands indicated by the argument
``relative=True``. Relative commands state the goal as offset relative to the current robot position.
As long as no custom reference frame is set, the offset has to be stated with regard to the base coordinate system.
The orientation is added as offset to the euler-angles.

Sequence
^^^^^^^^

.. code-block:: python

    sequence = Sequence()
    sequence.append(Lin(goal=Pose(position=Point(0.2, 0, 0.8)), vel_scale=0.1, acc_scale=0.1))
    sequence.append(Circ(goal=Pose(position=Point(0.2, -0.2, 0.8)), center=Point(0.1, -0.1, 0.8), acc_scale=0.4))
    sequence.append(Ptp(goal=pose_after_relative, vel_scale=0.2))

    sequencer.plan(sequence)

More examples can be adapted from the `pilz_robot_programming <https://github.com/PilzDE/pilz_industrial_motion/tree/melodic-devel/pilz_robot_programming>`_ repository.
