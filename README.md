# grid-test

Proof of concept to show that it is possible to layout multiple delivery slots in a schedule with CSS Grid.

Example Input
```
   startHour: 8,
      endHour: 22,
      deliverySlots: [
        { start: 8, end: 10 },
        { start: 8, end: 20 },
        { start: 10, end: 12 },
        { start: 10, end: 16 },
        { start: 15, end: 17 }
      ]
```
Will be dislplayed as:
![](https://github.com/sam-maass/grid-schedule-test/blob/master/example-image.png?raw=true)

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

