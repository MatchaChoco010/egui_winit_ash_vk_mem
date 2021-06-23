# egui_winit_ash_vk_mem

[![Latest version](https://img.shields.io/crates/v/egui_winit_ash_vk_mem.svg)](https://crates.io/crates/egui_winit_ash_vk_mem)
[![Documentation](https://docs.rs/egui_winit_ash_vk_mem/badge.svg)](https://docs.rs/egui_winit_ash_vk_mem)
![MIT](https://img.shields.io/badge/license-MIT-blue.svg)
![Apache](https://img.shields.io/badge/license-Apache-blue.svg)
[![egui version: 0.10.0](https://img.shields.io/badge/egui%20version-0.10.0-orange)](https://docs.rs/egui/0.10.0/egui/index.html)

This is the [egui](https://github.com/emilk/egui) integration crate for
[winit](https://github.com/rust-windowing/winit), [ash](https://github.com/MaikKlein/ash)
and [vk_mem](https://github.com/gwihlidal/vk-mem-rs).

# Example

```sh
cargo run --example example
```

```sh
cargo run --example user_texture
```

# Usage

```rust
fn main() -> Result<()> {
    let event_loop = EventLoop::new();
    // (1) Call Integration::new() in App::new().
    let mut app = App::new(&event_loop)?;

    event_loop.run(move |event, _, control_flow| {
        *control_flow = ControlFlow::Poll;
        // (2) Call integration.handle_event(&event).
        app.egui_integration.handle_event(&event);
        match event {
            Event::WindowEvent {
                event: WindowEvent::CloseRequested,
                ..
            } => *control_flow = ControlFlow::Exit,
            Event::WindowEvent {
                event: WindowEvent::Resized(_),
                ..
            } => {
                // (3) Call integration.recreate_swapchain(...) in app.recreate_swapchain().
                app.recreate_swapchain().unwrap();
            }
            Event::MainEventsCleared => app.window.request_redraw(),
            Event::RedrawRequested(_window_id) => {
                // (4) Call integration.begin_frame(), integration.end_frame(),
                // integration.context().tessellate(shapes), integration.paint(...) and
                // if let Some(cursor_icon) = Integration::egui_to_winit_cursor_icon(cursor_icon) {
                //     window.set_cursor_visible(true);
                //     window.set_cursor_icon()
                // } else {
                //     window.set_cursor_visible(false);
                // }
                // in app.draw().
                app.draw().unwrap();
            }
            _ => (),
        }
    })
}
// (5) Call integration.destroy() when drop app.
```

[Full example is in examples directory](https://github.com/MatchaChoco010/egui_winit_ash_vk_mem/tree/main/examples/example)

# License

MIT OR Apache-2.0
