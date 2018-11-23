# Reusable Component Style Guide

# Definitions

## Variable Style (variable style prop)

A style property that does not change the position or sizing of a component. Examples include, but are not limited to:

    background
    color
    font-size
    text-decoration

## Fundamental Style (fundamental style prop)

A style property that controls position and sizing of a component. Examples include, but are not limited to:

    display
    flex
    position
    z-index

Exceptions to this rule are for components that follow the same basic layout and position principles and only change based on one property. For example:

    const Component = styled.div`
        flex: ${({ width }) => width};
        height: 200px;
        position: relative;
    `;

    const Main = ({ width }) => (
        <Component width={2} />
        <Component width={1} />
    );

# Style Guide

- Style properties, flow types, and imports are to be alphabetized
    - The exception to this rule is optional props in flow; these should be alphabetized with the required props first, then the optional props
- Imports are to be organized in the following order:
    - Absolute imports (npm packages)
    - Components
    - Functions, objects
    - Types
- Prefer functional components over class based components

    - class Main extends React.Component {
    -   render() {
    -       return <div />
    -   }
    - }
    + const Main = () => <div />

- Components should be stateless ("dumb"), and wherever possible, represent some UI
- Props Types for each component are defined using `flow`

    type ComponentProps = {};

    const Component = (props: ComponentProps) => <div {...props} />;

- Prop Types should be as specific as possible (no generic `Function`, `Object`, or `any` types unless absolutely necessary)
- Props should be destructured before being passed into the component

    const Component = (props: ComponentProps) => {
        const {
            background,
            fontSize,
        } = props;
        return (
            <Container
                background="red"
                fontSize={16}
            />
        );
    }

- Use `styled-components` to create components and add styling
- If a component is made up of other components that will not be used elsewhere, the main component goes in a folder by the same name and all other `styled-components` go in a `styles.react.js` file to be imported to the main component

    |- component
    |  |- Component.react.js // exports Component
    |  |- styles.react.js

- Components in the `styles.react.js` folder are to be used only in the component directory that they are located. If a component in the file can/should be used elsewhere, it should be put into its own named file
- **Variable Styles** should be passed in as props and should come in through the theme object wherever possible.

    const Container = styled.div`
        ${({ theme }) => `
            background: theme.main.background;
            font-size: theme.font.regular;
        `};
    `;

- Parent components should not control the styling of their children

    const Wrapper = styled.div`
        background: ${({ background }) => background};
    `;

    const Container = ({ background }) => (
    -   <Wrapper background={background} />
    +   <Wrapper background="red" />
    );

    const Main = () => (
    -   <Container background="red" />
    +   <Container />
    );

- These should all have sensible defaults. UI Components should also not internally control their layout (eg, alignLeft props).
- Applicable **Fundamental Styles** should be hard-coded in the styled component or passed in as defaultProps

    const Component = styled.div`
        flex: 1;
        position: absolute;
    `;

    const Component = styled.div`
        flex: ${({ width }) => width};
        position: relative;
    `;

    Component.defaultProps = {
        width: 1,
    };