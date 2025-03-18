To build out complex systems you need tools to manage that complexity. Object orietned programming is one such method using the following set of elements:

- Primitives
- Combination
- Abstraction
- Patterns

[Introduction to Electrical Engineering](https://www.youtube.com/watch?v=CG4ihzTaGdM&list=PL9B24A6A9D5754E70&index=7)

## Signals and Systems

Use sonar sensors `currentDistance` to move a robot to `desiredDistance` from wall. To achieve the desired behavior you use a "proportional controller".

```python
class wallFinder(sm.SM):
    startState = None
    def getNextValues(self, state, inp):
        desiredDistance = 0.5
        currentDistance = inp.sonars[3]
        return (state, io.Action(fvel=?, rvel=0))
```
