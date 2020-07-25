<svelte:head>
  <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css">
</svelte:head>
<script>
    import {
      Collapse,
      Navbar,
      NavbarToggler,
      NavbarBrand,
      Nav,
      NavItem,
      NavLink,
      UncontrolledDropdown,
      DropdownToggle,
      DropdownMenu,
      DropdownItem,
      Container
    } from 'sveltestrap';
    import Button, {Label} from '@smui/button';
  
    let isOpen = false;
  
    function handleUpdate(event) {
      isOpen = event.detail.isOpen;
    }

  </script>

  
  <Navbar light expand="md">
    <NavbarBrand href="/">EventIn</NavbarBrand>
    <NavbarToggler on:click={() => (isOpen = !isOpen)} />
    <Collapse {isOpen} navbar expand="md" on:update={handleUpdate}>
      <Nav class="ml-auto" navbar>
        <NavItem>
          <NavLink href="#components/">Components</NavLink>
        </NavItem>
        <NavItem>
          <NavLink href="https://github.com/bestguy/sveltestrap">GitHub</NavLink>
        </NavItem>
        <UncontrolledDropdown nav inNavbar>
          <DropdownToggle nav caret>Options</DropdownToggle>
          <DropdownMenu right>
            <DropdownItem>Option 1</DropdownItem>
            <DropdownItem>Option 2</DropdownItem>
            <DropdownItem divider />
            <DropdownItem>Reset</DropdownItem>
          </DropdownMenu>
        </UncontrolledDropdown>
      </Nav>
    </Collapse>
  </Navbar>


<div class="main">
    <Container>
        <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Corrupti sunt dolor omnis sint et assumenda amet debitis distinctio? Facere saepe deserunt culpa sunt, at minus perferendis accusantium similique? Cum, nam!</p>
        <Button >
            <Label>Do Something</Label>
        </Button>
    </Container>
    
</div>

<style>
    
</style>
